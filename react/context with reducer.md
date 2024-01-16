# Context with Reducer

`b`

```typescript
type InitialStateType = {
  collapse: boolean;
  modalWidth: number;
  modalVisible: boolean;
  isCalled: boolean;
  currentTrack: CurrentTrack;
  localUser: LocalUser;
  remoteUser: RemoteUser;
  wsMessage: VideoWsMessage;
};
const initialState = {
  collapse: false,
  modalWidth: 894,
  modalVisible: false,
  isCalled: false,
  wsMessage: {
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
      second: "",
    },
    videoEventType: IMOnlineVideoEventType.NULL,
  },
  currentTrack: {
    KEY: CurrentTrackKey.RemoteUser,
    audioTrack: undefined,
    videoTrack: undefined,
    videoState: false,
    avator: "",
    userName: "",
    description: "",
  },

  localUser: {
    localAudioTrack: undefined,
    localVideoTrack: undefined,
    localUplinkNetworkQuality: 0,
    enabledVideoTrack: true,
    enabledAduioTrack: true,
  },
  remoteUser: {
    remoteUsersState: false,
    remoteUsersVolume: false,
    remoteUsersVideoEnabled: false,
    remoteUsersAudioEnabled: true,
    remoteUsers: [],
    remoteUplinkNetworkQuality: 0,
    name: "",
    avator: "",
  },
};
// 创建context对象
export const ModalContext = createContext<{
  state: InitialStateType;
  dispatch: React.Dispatch<Actions>;
}>({ state: initialState, dispatch: () => null });

// 定义每个state的reducer函数
const mainReducer = (state: InitialStateType, action: Actions) => ({
  collapse: collapseReducer(state.collapse, action),
  modalWidth: modalWidthReducer(state.modalWidth, action),
  isCalled: isCalledReducer(state.isCalled, action),
  currentTrack: currentTrackReducer(state.currentTrack, action),
  modalVisible: modalVisibleReducer(state.modalVisible, action),
  localUser: localUserReducer(state.localUser, action),
  remoteUser: remoteUserReducer(state.remoteUser, action),
  wsMessage: wsMessageReducer(state.wsMessage, action),
});

// provider组件
export const MoadlRtcProvider: React.FC = ({ children }) => {
  const [state, dispatch] = useReducer(mainReducer, initialState);
  return (
    <ModalContext.Provider value={{ state, dispatch }}>
      {children}
    </ModalContext.Provider>
  );
};
```

`reducer.ts`

```typescript
type ActionMap<M extends { [index: string]: any }> = {
  [Key in keyof M]: M[Key] extends undefined
    ? {
        type: Key;
      }
    : {
        type: Key;
        payload: M[Key];
      };
};
export enum ModalWidth {
  MiddleModal = 894,
  MinModal = 720,
  MaxModal = 1920,
}

type ModalWidthPayload = {
  [DispatchActionTypes.MinModal]: undefined;
  [DispatchActionTypes.MiddleModal]: undefined;
  [DispatchActionTypes.MaxModal]: undefined;
};
export type ModalWidthActions =
  ActionMap<ModalWidthPayload>[keyof ActionMap<ModalWidthPayload>];
export const modalWidthReducer = (state: number, action: Actions) => {
  switch (action.type) {
    case DispatchActionTypes.MinModal:
      return (state = ModalWidth.MinModal);
    case DispatchActionTypes.MiddleModal:
      return (state = ModalWidth.MiddleModal);
    case DispatchActionTypes.MaxModal:
      return (state = ModalWidth.MaxModal);
    default:
      return state;
  }
};

export type LocalUser = {
  localAudioTrack: ILocalAudioTrack | undefined;
  localVideoTrack: ILocalVideoTrack | undefined;
  localUplinkNetworkQuality: number;
  enabledVideoTrack: boolean;
  enabledAduioTrack: boolean;
};
type LocalUserPayload = {
  [DispatchActionTypes.LocalVideoTrack]: ILocalVideoTrack | undefined;
  [DispatchActionTypes.LocalAudioTrack]: ILocalAudioTrack | undefined;
  [DispatchActionTypes.LocalUplinkNetworkQuality]: number;
  [DispatchActionTypes.EnabledVideoTrack]: boolean;
  [DispatchActionTypes.EnabledAduioTrack]: boolean;
};
export type LocalUserActions =
  ActionMap<LocalUserPayload>[keyof ActionMap<LocalUserPayload>];
export const localUserReducer = (state: LocalUser, action: Actions) => {
  switch (action.type) {
    case DispatchActionTypes.LocalAudioTrack:
      return {
        ...state,
        localAudioTrack: action.payload,
      };
    case DispatchActionTypes.LocalVideoTrack:
      return {
        ...state,
        localVideoTrack: action.payload,
      };
    case DispatchActionTypes.EnabledAduioTrack:
      return {
        ...state,
        enabledAduioTrack: action.payload,
      };
    case DispatchActionTypes.EnabledVideoTrack:
      return {
        ...state,
        enabledVideoTrack: action.payload,
      };
    case DispatchActionTypes.LocalUplinkNetworkQuality:
      return {
        ...state,
        localUplinkNetworkQuality: action.payload,
      };
    default:
      return state;
  }
};

export type Actions =
  | CurrentTrackActions
  | ModalWidthActions
  | CollapseAction
  | LocalAudioTrackActions
  | LocalVideoTrackActions
  | RemoteUsersActions
  | EnabledAduioTrackActions
  | EnabledVideoTrackActions
  | LocalUplinkNetworkQualityActions
  | RemoteUplinkNetworkQualityReducerActions
  | ModalVisibleActions
  | RemoteUsersStateActions
  | RemoteUsersVolumeActions
  | RemoteUsersVideoVisibleActions
  | LocalUserActions
  | RemoteUserActions
  | IsCalledActions
  | WsMessageActions;
```

`ActionType.ts`

```typescript
export enum DispatchActionTypes {
  COLLAPSE = "COLLAPSE",
  MiddleModal = "MiddleModal",
  MinModal = "MinModal",
  MaxModal = "MaxModal",
  CurrentTrack = "CHANGE_CURRENTTRACK",
  LocalAudioTrack = "CHANGE_LOCALAUDIOTRACK",
  LocalVideoTrack = "CHANGE_LOCALVIDEOTRACK",
  RemoteUsers = "CHANGE_REMOTEUSERS",
  RemoteUsersInfo = "REMOTE_USER_INFO",
  EnabledAduioTrack = "ENABLE_ADUIOTRACK",
  EnabledVideoTrack = "ENABLE_VIDEOTRACK",
  LocalUplinkNetworkQuality = "LOCAL_UP_NETWORK_QUALITY",
  RemoteUplinkNetworkQuality = "REMOTE_UP_NETWORK_QUALITY",
  ModalVisible = "MODAL_VISIBLE",
  RemoteUsersState = "REMOTE_USERS_STATE",
  RemoteUsersVolume = "REMOTE_USERS_VOLUME",
  RemoteUsersVideoEnabled = "REMOTE_USER_ENABLED",
  RemoteUsersAudioEnabled = "REMOTE_USER_AUDIO_ENABLED",
  IsCalled = "IsCalled",
  WsMessage = "WsMessage",
}
export enum CurrentTrackKey {
  RemoteUser = "REMOTE_USER",
  LocalUser = "LOCAL_USER",
}
```

`useAgora.tsx`

```typescript
import {
  useState,
  useEffect,
  useContext,
  useRef,
  useCallback,
  useMemo,
} from "react";
import type {
  IAgoraRTCClient,
  IAgoraRTCRemoteUser,
  ILocalVideoTrack,
  ILocalAudioTrack,
  NetworkQuality,
} from "agora-rtc-sdk-ng";
import AgoraRTC from "agora-rtc-sdk-ng";
import { message } from "antd";
import _, { lowerFirst } from "lodash";

import { useService } from "@ali/servos-ui";
import { useApplicationMDI } from "@ali/servos-ui";

import { useApi } from "@/hooks";
import { VideoRecordService } from "@/core/services/VideoRecordService/VideoRecordService";
import type { OnlineAppDocument } from "@/core";
import { VideoDescription, VideoRecordStatus } from "@/constants";
import { IMOnlineVideoEventType } from "@/generated/online";
import type { VideoEventData } from "@/types/wsType";

import { DispatchActionTypes, CurrentTrackKey } from "../context/ActionType";
import { ModalContext } from "../context/context";

type UseAgoraReturn = {
  leave: Function;
  join: Function;
  ms: MediaStream | undefined;
  setLocalVideoEnabled: (enabled: boolean) => void;
  setLocalAudioEnabled: (enabled: boolean) => void;
  returnResources: () => void;
};
type InviteReturn = {
  uid: number;
  videoConversationId: number;
  appId: string;
  cname: string;
  token: string;
  queue: number;
  [K: string]: any;
};
type UseRTCApiReturn = {
  videoApiInvite: () => Promise<InviteReturn | boolean>;
  videoApiLeave: (k?: boolean) => Promise<void>;
  videoApiAnswer: () => Promise<boolean | unknown>;
  videoReconnect: () => Promise<void>;
};

export const useRTCApi = (): UseRTCApiReturn => {
  const request = useApi();
  const { state } = useContext(ModalContext);
  const { active } = useApplicationMDI<OnlineAppDocument>();
  const { wsMessage, remoteUser } = state;
  const UserId = useMemo(() => active?.id, [active]);
  const videoApiInvite = useCallback(async () => {
    const res = await request(
      "mopen.merge.xixikf.service.support.video.changestatus",
      {
        videoEventType: VideoRecordStatus.INVITE,
        touchId: UserId,
      }
    );
    const result = res?.value as InviteReturn;
    if (!_.isEmpty(result)) {
      return Object.assign(result, { touchId: UserId });
    }
    message.error("系统异常");
    return false;
  }, [UserId]);
  const videoApiAnswer = useCallback(async () => {
    if (_.isEmpty(wsMessage)) return false;
    console.log("videoApiAnswer", wsMessage);
    const res = await request(
      "mopen.merge.xixikf.service.support.video.changestatus",
      {
        videoEventType: VideoRecordStatus.ANSWER,
        touchId: wsMessage.videoEventData.touchId,
        videoConversationId: wsMessage.videoEventData.videoConversationId,
        videoToken: wsMessage.videoEventData.token,
      }
    );
    return res;
  }, [wsMessage]);

  async function videoApiLeave(isHolding?: boolean) {
    const ConversationId = wsMessage.videoEventData.videoConversationId;
    const remoteUsersState = remoteUser.remoteUsersState;
    if (!ConversationId) return;
    await request("mopen.merge.xixikf.service.support.video.changestatus", {
      videoEventType: remoteUsersState
        ? VideoRecordStatus.HANGUP
        : VideoRecordStatus.CANCEL,
      touchId: state.wsMessage.videoEventData.touchId,
      videoConversationId: Number(ConversationId),
      isHolding: isHolding ? true : false,
    });
  }

  const videoReconnect = useCallback(async () => {
    await request("mopen.merge.xixikf.service.support.video.changestatus", {
      videoEventType: VideoRecordStatus.RECONNECT,
    });
  }, []);
  return {
    videoApiInvite,
    videoApiLeave,
    videoApiAnswer,
    videoReconnect,
  };
};

export default function useAgora(
  client: IAgoraRTCClient | undefined
): UseAgoraReturn {
  const { state, dispatch } = useContext(ModalContext);
  const { localUser, currentTrack, remoteUser } = state;
  const { active } = useApplicationMDI<OnlineAppDocument>();
  const [ms, setMediaStream] = useState<MediaStream | undefined>(undefined);
  const videoRecordService = useService<VideoRecordService>(VideoRecordService);
  const { videoApiInvite, videoApiLeave } = useRTCApi();
  let statusInterval: any;
  async function join(vidoeInfo: VideoEventData) {
    try {
      if (!client) return;
      if (!vidoeInfo) {
        return dispatch({
          type: DispatchActionTypes.ModalVisible,
          payload: false,
        });
      }
      const clientUid = await client.join(
        vidoeInfo.appId,
        vidoeInfo.cname,
        vidoeInfo.token
      );
      const audioTrack = await AgoraRTC.createMicrophoneAudioTrack();
      dispatch({
        type: DispatchActionTypes.LocalAudioTrack,
        payload: audioTrack,
      });
      const videoTrack = await AgoraRTC.createCameraVideoTrack();
      dispatch({
        type: DispatchActionTypes.LocalVideoTrack,
        payload: videoTrack,
      });
      await client.publish(Object.values({ videoTrack, audioTrack }));
    } catch (error) {
      message.error("系统异常");
      leave(VideoRecordStatus.INTERRUPT);
      console.log("useAgora-join---->", error);
    }
  }
  async function leave(
    EventType: IMOnlineVideoEventType | VideoRecordStatus,
    isHolding?: boolean
  ) {
    console.log("触发挂断逻辑", EventType, isHolding, state);
    if (
      EventType === IMOnlineVideoEventType.HANGUP ||
      EventType === IMOnlineVideoEventType.CANCEL ||
      EventType === VideoRecordStatus.INTERRUPT
    ) {
      console.log(remoteUser);

      await videoApiLeave(isHolding);
    }
    if (localUser.localAudioTrack) {
      localUser.localAudioTrack.stop();
      localUser.localAudioTrack.close();
      dispatch({
        type: DispatchActionTypes.LocalAudioTrack,
        payload: undefined,
      });
    }
    if (localUser.localVideoTrack) {
      localUser.localVideoTrack.stop();
      localUser.localVideoTrack.close();
      dispatch({
        type: DispatchActionTypes.LocalVideoTrack,
        payload: undefined,
      });
    }
    await client?.leave();
    dispatch({ type: DispatchActionTypes.ModalVisible, payload: false });
    dispatch({ type: DispatchActionTypes.RemoteUsersState, payload: false });
    dispatch({ type: DispatchActionTypes.RemoteUsers, payload: [] });
    dispatch({
      type: DispatchActionTypes.RemoteUplinkNetworkQuality,
      payload: 0,
    });
    dispatch({
      type: DispatchActionTypes.RemoteUsersVideoEnabled,
      payload: false,
    });
    dispatch({
      type: DispatchActionTypes.RemoteUsersAudioEnabled,
      payload: true,
    });
    dispatch({ type: DispatchActionTypes.EnabledAduioTrack, payload: true });
    dispatch({ type: DispatchActionTypes.EnabledVideoTrack, payload: true });
    dispatch({
      type: DispatchActionTypes.LocalUplinkNetworkQuality,
      payload: 0,
    });
    dispatch({ type: DispatchActionTypes.IsCalled, payload: false });
    dispatch({
      type: DispatchActionTypes.CurrentTrack,
      payload: {
        ...currentTrack,
        audioTrack: undefined,
        videoTrack: undefined,
      },
    });
    videoRecordService.emit("setVideoRecordStatus", VideoRecordStatus.HANGUP);
  }

  async function returnResources() {
    if (localUser.localAudioTrack) {
      localUser.localAudioTrack.stop();
      localUser.localAudioTrack.close();
      dispatch({
        type: DispatchActionTypes.LocalAudioTrack,
        payload: undefined,
      });
    }
    if (localUser.localVideoTrack) {
      localUser.localVideoTrack.stop();
      localUser.localVideoTrack.close();
      dispatch({
        type: DispatchActionTypes.LocalVideoTrack,
        payload: undefined,
      });
    }
  }

  function trackToStream(): void {
    if (currentTrack?.videoTrack) {
      const mediaStreamTrack = currentTrack?.videoTrack.getMediaStreamTrack();
      // setTimeout(() => {
      const mediaStream = new MediaStream([mediaStreamTrack]);
      setMediaStream(mediaStream);
      // }, 200);
    }
  }

  function setLocalVideoEnabled(enabled: boolean = true): void {
    if (localUser.localVideoTrack) {
      localUser.localVideoTrack.setEnabled(enabled);
    }
    dispatch({ type: DispatchActionTypes.EnabledVideoTrack, payload: enabled });
    if (currentTrack.KEY === CurrentTrackKey.LocalUser) {
      const video = currentTrack.videoTrack;
      dispatch({
        type: DispatchActionTypes.CurrentTrack,
        payload: {
          ...currentTrack,
          videoTrack: video,
          videoState: enabled,
        },
      });
    }
    message.info(`摄像头已${enabled ? "开启" : "关闭"}`);
  }

  function setLocalAudioEnabled(enabled: boolean = true): void {
    if (localUser.localAudioTrack) {
      localUser.localAudioTrack.setEnabled(enabled);
    }
    dispatch({ type: DispatchActionTypes.EnabledAduioTrack, payload: enabled });
    message.info(`麦克风已${enabled ? "开启" : "关闭"}`);
  }

  useEffect(() => {
    trackToStream();
  }, [currentTrack]);

  useEffect(() => {
    if (!client) return;
    dispatch({
      type: DispatchActionTypes.RemoteUsers,
      payload: Array.from(client.remoteUsers),
    });
    const handleUserPublished = async (
      user: IAgoraRTCRemoteUser,
      mediaType: "audio" | "video"
    ) => {
      await client.subscribe(user, mediaType);
      videoRecordService.emit("setVideoRecordStatus", VideoRecordStatus.ANSWER);
      // toggle rerender while state of remoteUsers changed.
      dispatch({
        type: DispatchActionTypes.RemoteUsers,
        payload: Array.from(client.remoteUsers),
      });
      dispatch({ type: DispatchActionTypes.RemoteUsersState, payload: true });
      dispatch({
        type: DispatchActionTypes.RemoteUsersVideoEnabled,
        payload: user.hasVideo,
      });
      dispatch({
        type: DispatchActionTypes.RemoteUsersAudioEnabled,
        payload: user.hasAudio,
      });
      if (currentTrack.KEY === CurrentTrackKey.RemoteUser) {
        dispatch({
          type: DispatchActionTypes.CurrentTrack,
          payload: {
            ...currentTrack,
            videoState: user.hasVideo,
            description: user.hasVideo ? "" : VideoDescription.EnableVideotrack,
            audioTrack: client.remoteUsers[0].audioTrack,
            videoTrack: client.remoteUsers[0].videoTrack,
            // userName: active.userName,
            // avator: active.avator,
          },
        });
      }
    };
    const handleUserUnpublished = (user: IAgoraRTCRemoteUser) => {
      dispatch({
        type: DispatchActionTypes.RemoteUsers,
        payload: Array.from(client.remoteUsers).filter(
          (item) => item._video_muted_ === false
        ),
      });
      if (!user.hasVideo) {
        dispatch({
          type: DispatchActionTypes.RemoteUsersVideoEnabled,
          payload: user.hasVideo,
        });
      }
      if (!user.hasAudio) {
        dispatch({
          type: DispatchActionTypes.RemoteUsersAudioEnabled,
          payload: user.hasAudio,
        });
      }
      if (currentTrack.KEY === CurrentTrackKey.RemoteUser && !user.hasVideo) {
        dispatch({
          type: DispatchActionTypes.CurrentTrack,
          payload: {
            ...currentTrack,
            description: VideoDescription.EnableVideotrack,
            videoState: user.hasVideo,
            // userName: active.userName,
            // avator: active.avator,
          },
        });
      }
    };
    const handleUserJoined = (user: IAgoraRTCRemoteUser) => {
      dispatch({
        type: DispatchActionTypes.RemoteUsers,
        payload: Array.from(client.remoteUsers),
      });
    };
    const handleUserLeft = (user: IAgoraRTCRemoteUser) => {
      dispatch({
        type: DispatchActionTypes.RemoteUsers,
        payload: Array.from(client.remoteUsers),
      });
      // dispatch({ type: DispatchActionTypes.RemoteUsersState, payload: false });
      dispatch({
        type: DispatchActionTypes.RemoteUsersVideoEnabled,
        payload: false,
      });
    };

    client.on("user-published", handleUserPublished);
    client.on("user-unpublished", handleUserUnpublished);
    client.on("user-joined", handleUserJoined);
    client.on("user-left", handleUserLeft);

    return () => {
      client.off("user-published", handleUserPublished);
      client.off("user-unpublished", handleUserUnpublished);
      client.off("user-joined", handleUserJoined);
      client.off("user-left", handleUserLeft);
    };
  }, [currentTrack, client]);

  useEffect(() => {
    if (!client) return;
    // ...todo  networkQuality.uplinkNetworkQuality != state.localUplinkNetworkQuality
    const handleNetworkQuality = (networkQuality: NetworkQuality) => {
      if (
        networkQuality.uplinkNetworkQuality !==
        localUser.localUplinkNetworkQuality
      ) {
        dispatch({
          type: DispatchActionTypes.LocalUplinkNetworkQuality,
          payload: networkQuality.uplinkNetworkQuality,
        });
      }
    };
    client.on("network-quality", handleNetworkQuality);
    return () => {
      client.off("network-quality", handleNetworkQuality);
    };
  }, [client, localUser.localUplinkNetworkQuality]);

  // ...todo  networkQuality.uplinkNetworkQuality != state.localUplinkNetworkQuality
  const fetchNetWorkQuality = () => {
    try {
      const remoteNetworkQuality = client!.getRemoteNetworkQuality();
      if (Object.values(remoteNetworkQuality).length < 1) return;
      const uplinkNetworkQuality =
        Object.values(remoteNetworkQuality)[0].uplinkNetworkQuality;
      if (uplinkNetworkQuality !== remoteUser.remoteUplinkNetworkQuality) {
        dispatch({
          type: DispatchActionTypes.RemoteUplinkNetworkQuality,
          payload: Object.values(remoteNetworkQuality)[0].uplinkNetworkQuality,
        });
      }
    } catch (error) {
      console.log("fetchNetWorkQuality", error);
    }
  };
  useEffect(() => {
    if (!remoteUser.remoteUsersState) return;
    statusInterval = setInterval(fetchNetWorkQuality, 3000);
    return () => {
      clearInterval(statusInterval);
    };
  }, [remoteUser.remoteUplinkNetworkQuality, remoteUser.remoteUsersState]);

  return {
    ms,
    leave,
    join,
    setLocalVideoEnabled,
    setLocalAudioEnabled,
    returnResources,
  };
}
```

改进：对 ` const { state, dispatch } = useContext(ModalContext)`进行优化

```typescript
const useModalContext = () => {
  const { state, dispatch } = useContext(ModalContext);
  const dispatchHandle = useCallback(() => {
    dispatch({});
  }, [state]);
  return { state, dispatchHandle, dispatch };
};
```
