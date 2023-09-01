使用mobx对Agora进行重新封装 （前面使用context和reducer）

`Agora.ts`

```typescript
// import
// // AgoraRTC,
// {  } from "agora-rtc-sdk-ng";
import type {
  IAgoraRTCClient,
  IAgoraRTCRemoteUser,
  ILocalVideoTrack,
  ILocalAudioTrack,
  UID,
  IMicrophoneAudioTrack,
  IRemoteVideoTrack,
  IAgoraRTC,
} from "agora-rtc-sdk-ng";
import { useContext } from "react";
import {
  CallingStatus,
  CurrentTrack,
  CurrentTrackKey,
  IMOnlineVideoEventType,
  LocalUser,
  RemoteUser,
  VideoChatPageStatus,
  VideoDescription,
  VideoWsMessage,
} from "./type";
import {
  makeObservable,
  makeAutoObservable,
  observable,
  computed,
  action,
} from "mobx";

const initialLocalUser = {
  localAudioTrack: undefined,
  localVideoTrack: undefined,
  localUplinkNetworkQuality: 0,
  enabledVideoTrack: true,
  enabledAduioTrack: true,
};
const initialRemoteUser = {
  remoteUsersState: false,
  remoteUsersVolume: false,
  remoteUsersVideoEnabled: false,
  remoteUsersAudioEnabled: true,
  remoteUsers: [],
  remoteUplinkNetworkQuality: 0,
  name: "",
  avator: "",
};
const initialVideoWsMessage = {
  videoEventData: {
    uid: 0,
    videoConversationId: 0,
    appId: "",
    token: "",
    cname: "",
    queue: 0,
    touchId: "",
    customerName: "",
    avator: "",
  },
  videoEventType: IMOnlineVideoEventType.NULL,
};
const initialCurrentTrack = {
  KEY: CurrentTrackKey.RemoteUser,
  audioTrack: undefined,
  videoTrack: undefined,
  videoState: false,
  avator: "",
  userName: "",
  description: "",
};

class AgoraRtc {
  public client!: IAgoraRTCClient;
  public localMediaStreamTrack!: MediaStreamTrack;
  public localMediaStream!: MediaStream;
  public AgoraRTC!: IAgoraRTC;
  private initialization = false; //
  callStatus: CallingStatus = CallingStatus.NULL;
  videoChatPageStatus!: VideoChatPageStatus;
  VideoWsMessage: VideoWsMessage = initialVideoWsMessage;
  localUser: LocalUser = initialLocalUser;
  remoteUser: RemoteUser = initialRemoteUser;
  currentTrack: CurrentTrack = initialCurrentTrack;
  count: number = 0;

  constructor() {
    // import("agora-rtc-sdk-ng").then((res) => {
    // });
    // if (!window.__ARTC__) return;
    // console.log("constructor");
    // this.AgoraRTC = window.__ARTC__;
    // this.init();
    // makeAutoObservable(this);
    // window.onbeforeunload = () => {
    //   this.returnResources();
    // };
  }
  public get getLocalMediaStream() {
    return this.localMediaStream;
  }

  render = () => {
    if (!window.__ARTC__ || this.initialization) return;
    console.log("constructor");
    this.initialization = true;
    this.AgoraRTC = window.__ARTC__;
    this.init();
    makeAutoObservable(this);
    window.onbeforeunload = () => {
      this.returnResources();
    };
  };

  setLocalMediaStream = (
    track: ILocalVideoTrack | IRemoteVideoTrack | undefined
  ) => {
    if (!track) return;
    this.localMediaStreamTrack = track.getMediaStreamTrack();
    this.localMediaStream = new MediaStream([this.localMediaStreamTrack]);
  };

  public init() {
    this.client = this.AgoraRTC.createClient({
      codec: "vp8",
      mode: "rtc",
    });

    this.client.on("user-published", this.handleUserPublished);
    this.client.on("user-unpublished", this.handleUserUnpublished);
    this.client.on("user-joined", this.handleUserJoined);
    this.client.on("user-left", this.handleUserLeft);
  }
  setCount = (count: number) => {
    console.log("setCount", count);
    this.count = count;
  };
  setVideoWsMessage(value: Partial<VideoWsMessage>) {
    this.VideoWsMessage = {
      ...this.VideoWsMessage,
      ...value,
    };
  }

  setCallStatus(value: CallingStatus) {
    this.callStatus = value;
  }
  setVideoChatPageStatus(value: VideoChatPageStatus) {
    this.videoChatPageStatus = value;
  }

  public async createLocalTrack() {
    this.localUser.localAudioTrack =
      await this.AgoraRTC.createMicrophoneAudioTrack();
    this.localUser.localVideoTrack =
      await this.AgoraRTC.createCameraVideoTrack();
  }

  setCurrentTrack = (parameter: Partial<CurrentTrack>) => {
    const { audioTrack, videoTrack, KEY } = parameter;
    if (videoTrack) {
      this.setLocalMediaStream(videoTrack);
      // this.localMediaStreamTrack = videoTrack.getMediaStreamTrack();
      // this.localMediaStream = new MediaStream([this.localMediaStreamTrack]);
    }
    this.currentTrack = {
      ...this.currentTrack,
      ...parameter,
    };
  };

  setLocalVideoEnabled(enabled: boolean) {
    if (this.localUser.localVideoTrack) {
      this.localUser.localVideoTrack?.setEnabled(enabled);
    }
    if (this.currentTrack.KEY === CurrentTrackKey.LocalUser) {
      this.currentTrack.videoState = enabled;
      if (enabled) {
        console.log("转换stream");
        const track = this.localUser.localVideoTrack;
        this.setLocalMediaStream(track);
      }
    }
    this.localUser.enabledVideoTrack = enabled;
  }
  setLocalAudioEnabled(enabled: boolean) {
    if (this.localUser.localAudioTrack) {
      this.localUser.localAudioTrack?.setEnabled(enabled);
    }
    this.localUser.enabledAduioTrack = enabled;
  }
  public async check4Permissions(): Promise<boolean> {
    try {
      this.AgoraRTC.getDevices().then((devices) => {
        const audioDevices = devices.filter(function (device) {
          return device.kind === "audioinput";
        });
        const videoDevices = devices.filter(function (device) {
          return device.kind === "videoinput";
        });
      });
      return Promise.resolve(true);
    } catch (error) {
      return Promise.reject(false);
    }
  }
  public returnResources = () => {
    console.log("returnResources");

    if (this.localUser.localAudioTrack) {
      this.localUser.localAudioTrack.stop();
      this.localUser.localAudioTrack.close();
    }
    if (this.localUser.localVideoTrack) {
      this.localUser.localVideoTrack.stop();
      this.localUser.localVideoTrack.close();
    }
    this.client.leave();
  };
  public async join(
    appid: string,
    channel: string,
    token: string | null,
    uid?: UID | null
  ): Promise<UID | boolean> {
    console.log("join", this.client);

    if (!this.client) return false;
    console.log(appid, channel, token);

    if (!appid || !channel || !token) return false;
    try {
      const clientUid = await this.client.join(appid, channel, token, uid);
      this.localUser.localAudioTrack =
        await this.AgoraRTC.createMicrophoneAudioTrack();
      this.localUser.localVideoTrack =
        await this.AgoraRTC.createCameraVideoTrack();
      this.localUser.localVideoTrack?.setEnabled(
        this.localUser.enabledVideoTrack
      );
      this.localUser.localAudioTrack?.setEnabled(
        this.localUser.enabledAduioTrack
      );
      await this.client.publish(
        Object.values({
          videoTrack: this.localUser.localVideoTrack!,
          audioTrack: this.localUser.localAudioTrack!,
        })
      );

      return clientUid;
    } catch (error) {
      console.log("join", error);

      return false;
    }
  }
  async leave(): Promise<void> {
    this.localUser.localAudioTrack?.stop();
    this.localUser.localAudioTrack?.close();
    this.localUser.localVideoTrack?.stop();
    this.localUser.localVideoTrack?.close();
    this.client.leave();
    this.callStatus = CallingStatus.NULL;
    this.videoChatPageStatus = VideoChatPageStatus.Null;
    this.localUser = initialLocalUser;
    this.remoteUser = initialRemoteUser;
    this.currentTrack = initialCurrentTrack;
    this.VideoWsMessage = initialVideoWsMessage;
    // this.client.off("user-published", this.handleUserPublished);
    // this.client.off("user-unpublished", this.handleUserUnpublished);
    // this.client.off("user-joined", this.handleUserJoined);
    // this.client.off("user-left", this.handleUserLeft);
  }

  private handleUserPublished = async (
    user: IAgoraRTCRemoteUser,
    mediaType: "audio" | "video"
  ) => {
    await this.client.subscribe(user, mediaType);
    this.remoteUser.remoteUsers = Array.from(this.client.remoteUsers);
    this.remoteUser.remoteUsersState = true;
    this.remoteUser.remoteUsersAudioEnabled = user.hasAudio;
    this.remoteUser.remoteUsersVideoEnabled = user.hasVideo;
    if (this.currentTrack.KEY === CurrentTrackKey.RemoteUser) {
      this.currentTrack.videoState = user.hasVideo;
      this.currentTrack.description = user.hasVideo
        ? ""
        : VideoDescription.EnableVideotrack;
      this.currentTrack.audioTrack = this.client.remoteUsers[0].audioTrack;
      this.currentTrack.videoTrack = this.client.remoteUsers[0].videoTrack;
      this.setLocalMediaStream(this.client.remoteUsers[0].videoTrack);
    }
  };
  private handleUserUnpublished = (user: IAgoraRTCRemoteUser) => {
    if (!user.hasVideo) {
      this.remoteUser.remoteUsersVideoEnabled = user.hasVideo;
    }
    if (!user.hasAudio) {
      this.remoteUser.remoteUsersAudioEnabled = user.hasAudio;
    }
    if (
      this.currentTrack.KEY === CurrentTrackKey.RemoteUser &&
      !user.hasVideo
    ) {
      this.currentTrack.description = VideoDescription.EnableVideotrack;
      this.currentTrack.videoState = user.hasVideo;
    }
  };
  private handleUserJoined = (user: IAgoraRTCRemoteUser) => {
    this.remoteUser.remoteUsers = Array.from(this.client.remoteUsers);
  };
  private handleUserLeft = (user: IAgoraRTCRemoteUser) => {
    this.remoteUser.remoteUsers = Array.from(this.client.remoteUsers);
    this.remoteUser.remoteUsersVideoEnabled = false;
  };
}
// export type AgoraRTCType = typeof AgoraRTC;
export type AgoraInstanceType = InstanceType<typeof AgoraRtc>;
export const Agora: AgoraInstanceType = new AgoraRtc();

```

`AgoraContext.tsx`

```typescript
import { Agora as AgoraInstance, AgoraInstanceType } from "@/Agora";
import { createContext, useState } from "react";

type Props = {
  children: React.ReactNode;
};

export const AgoraContext = createContext<AgoraInstanceType>(AgoraInstance);
export const AgoraProvider = ({ children }: Props) => {
  // const [Agora, setAgora] = useState(AgoraInstance);
  // const proxy = new Proxy(Agora, {
  //   set: function (target, propKey, value, receiver) {
  //     console.log(`setting,key ${propKey}! value ${value}`);
  //     console.log("pre-target", target);

  //     const newAgora = Reflect.set(target, propKey, value, receiver);
  //     console.log("modify-target", target);

  //     setAgora({ ...target });
  //     return newAgora;
  //   },
  // });
  return (
    <AgoraContext.Provider value={AgoraInstance}>
      {children}
    </AgoraContext.Provider>
  );
};

```
