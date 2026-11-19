# Spotify

Track info and play/pause/next/shuffle/repeat come from Windows media controls and work with any player, Spotify included, without any setup. Connecting a Spotify account adds, while Spotify is playing:

- a heart next to the title to save the track to your Liked Songs or remove it;
- a device button in the app volume row to move playback to another Spotify Connect device (phone, speaker, another computer);
- the app volume slider driving Spotify's own volume on the active device, this computer included, whenever that device lets Spotify change its volume.

## Setup

Spotify limits apps in Development Mode to 5 users and requires their owner to have Premium, so each user creates their own app:

1. Sign in to <https://developer.spotify.com/dashboard> with a Premium account and create an app.
2. Tick **Web API** and add the redirect URI `http://127.0.0.1:27211/callback` (also shown in **Settings › Spotify**).
3. Paste the app's **Client ID** into **Settings › Spotify**, click **Connect** and accept in the browser.

To use the same app with another Spotify account, add that account under **User Management** in the dashboard.

## Notes

- There is no client secret to enter. The login is stored in `%APPDATA%\dev.clems.notch\spotify.json`, and **Disconnect** deletes it. Changing the Client ID disconnects the account.
- Notch only calls Spotify on track change, when the notch opens (at most every 10 seconds) and when you use one of these controls, since Development Mode quotas are shared across all of your apps.
- The heart only appears once Spotify reports the same track as Windows, so a like never lands on the previous track.
- Port `27211` must be free while connecting.
