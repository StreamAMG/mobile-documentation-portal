#  StreamSDK React Native PlayKit Wrapper

When dealing with the elements of the SDK via React Native, it is best to use a middle-man style common wrapper to help align the native SDKs and provide a simple, reusable code base. The first component of the wrapper is PlayKit

## Installing the PlayKit wrapper


## Getting Started

## Sending data to the PlayKit wrapper

The data is sent to the wrapper in the form of a single React Native structure that contains all the information needed to instantiate the player and play a media item:

``` JS
export type StreamAMGPlayKitProps = {
  loadMedia: StreamAMGPlayKitLoadMedia; // See 'StreamAMGPlayKitLoadMedia' immediately below
  width: number; // Width of the player window in pixels
  height: number; // Height of the player window in pixels
  close: boolean; // Whether the player should be force closed (see 'Closing the Player')
  style: object;
  autoPlay: boolean; // Should the player start automatically
  playing: boolean; 
  portrait: boolean; // Is the player showing on a Portait orientation
  onListenerEvents: (event: any) => void; // See 'Events'
  spoilerMode: boolean; // Should the player run in Spoiler Mode
  watchFromStart: boolean; // Should a live event play from the start or from 'now'
};
```

StreamAMGPlayKitLoadMedia is a single dictionary, defined as

``` JS
export interface StreamAMGPlayKitLoadMedia {
  token?: string; // A validK KS token (nil or "" will ignore the KS)
  entryId: string; // The entry ID of the media
  partnerId: string; // The client's partner ID
  serverUrl: string; // The server the media is hosted on
  code?: string; // See 'Adding an analytics service'
  user?: string; // See 'Adding an analytics service'
  title?: string; // See 'Adding an analytics service'
  parameters?: YouboraExtra; // See 'Adding an analytics service'
}
```

### Adding an analytics service

Currently the wrapper only supports the use of the Youbora analytics service. To use Youbora Analytics, the only requirement is for a Youbora Account Code. If this is provided (as part of the 'StreamAMGPlayKitLoadMedia' data) then Youbora will run and deliver data to the specified code.

The following data can be supplied for Youbora:

``` JS
  code?: string; // Youbora 'account code'
  user?: string; // An anonymous user ID
  title?: string; // The title of the currently playing Media
  parameters?: YouboraExtra; // See 'YouboraExtra' below
```

Extra Youbora parameters can be added with an array of the following structure:

``` JS
export interface YouboraExtra {
  [key: string]: string;
}
```

Key is an integer from 1 to 20 as a String
the value is a String

["1"]: "Wembley"
["2"]: "FA Cup"
etc

The Key integers must match up to the parameters (dimensions) in the Youbora dashboard for any data to be recorded, in this instance Dimension1 might be set as 'Stadium' and Dimention2 as 'Competition', etc

### Closing the Player

The player should always be closed and re-instantiated before new media is loaded (or after the currently playing media page has been closed)

this is achieved by

It should be noted that if you do not close the player correctly, it may stay playing in the background

### Events

PlayKit sends events to the wrapper, which in turn will repeat those events to a listening React Native app. 

To set this up, you should....

Events that PlayKit can report are errors :
```
For errors, you get the error code sent to "errorCode" and a message sent to "errorMessage"
SOURCE_ERROR(7000) - The error occured loading data from MediaSource.

RENDERER_ERROR(7001) - The error occured in a renderer.

UNEXPECTED(7002) - If in runtime any unexpected error occurs.

SOURCE_SELECTION_FAILED(7003) - The error occured to get the source from SourceSelector.

FAILED_TO_INITIALIZE_PLAYER(7004) - The error occured when failed to initilize PlayerEngine.

DRM_ERROR(7005) - In case device does not support widevine modular or license is expired.

TRACK_SELECTION_FAILED(7006) - The error occured if track selection is not possible in TrackSelectionHelper.

LOAD_ERROR(7007) - In case, media is not loaded in any of the MediaSource.

OUT_OF_MEMORY(7008)

REMOTE_COMPONENT_ERROR(7009)

TIMEOUT(7010){{}}
```

And Play events:

```
For the player state, you get a state sent to "state"
 
The states are:
 
‘play’ - if play has been requested
 
‘playing’ -  if playback has started
 
‘pause’ - if pause / stop has been requested
 
and ‘ended’ - if it has stopped
```

# Change Log

All notable changes to this project will be documented in this section.

