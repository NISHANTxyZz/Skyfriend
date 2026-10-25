# SkyFriends 1.0.0

A Java friend-system plugin for Paper/Spigot 1.21.4, built with Java 21 and SQLite.

## Included
- `/friend` GUI and help
- add, accept, deny, remove, removeall confirmation
- paginated friend lists and request lists
- best-friend toggle and private nicknames
- request and notification privacy toggles
- ignore add/remove/list
- clickable request Accept / Deny / Ignore messages
- SQLite persistence across restarts
- join/leave notifications
- admin reload and info commands
- configurable messages, request expiry, friend cap, GUI titles
- Gradle build with SQLite JDBC shaded into the output JAR

## Important notes
- The project targets the Spigot API 1.21.4 and should run on Paper 1.21.4.
- Chat click events use Spigot's Bungee chat API for compatibility.
- SQLite calls are synchronized and currently executed synchronously. This is simple and suitable for small test servers, but for large networks migrate database operations to an async service before production.
- Friend requests can be sent to players whose UUID/name has previously been recorded by the server, or to online players. Offline players never seen by this server cannot be resolved by name without a separate Mojang profile lookup.
- Privacy and ignore controls govern friend requests; ignore does not block Minecraft chat by itself.
- GUI left-click toggles best-friend status; right-click removes the friendship.
- `/friend removeall confirm` removes all friendships and pending requests for the sender.
- For multi-server friends, add a proxy-aware shared database/message layer; this build is single-server.

## Build in Termux
Install a JDK and Git, then install Gradle (or use the included wrapper if you add one):
```sh
pkg update
pkg install openjdk-21 gradle git
java -version
gradle --version
```

Copy/extract the project, enter the project directory, and build:
```sh
cd SkyFriends
gradle clean build
```

The shaded plugin JAR will be:
`build/libs/SkyFriends-1.0.0.jar`

Upload that JAR to `plugins/` on your test Paper/Spigot server and restart the server. Do not put the unshaded JAR in the server.

## Aternos
If your Aternos server allows uploading custom plugin JARs for your selected software/version, upload `SkyFriends-1.0.0.jar` through the server's file manager. Otherwise test on a local Paper/Spigot server or another host that permits custom JAR uploads.

## Commands
- `/friend` — open friends menu
- `/friend add <player>`
- `/friend accept <player>`
- `/friend deny <player>`
- `/friend remove <player>`
- `/friend list [page]`
- `/friend requests [page]`
- `/friend best <player>`
- `/friend nickname <player> <nickname|clear>`
- `/friend toggle`
- `/friend notifications`
- `/friend removeall confirm`
- `/friend menu`
- `/ignore add <player>`
- `/ignore remove <player>`
- `/ignore list`
- `/privacy`
- `/friendadmin reload`
- `/friendadmin info <player>`

## Test checklist
1. Start the server and check for `SkyFriends enabled`.
2. Join with two accounts (Alice and Bob).
3. Run `/friend add Bob`; confirm Bob gets clickable actions.
4. Accept and check both players' friend lists.
5. Remove the friendship and verify it disappears for both players.
6. Test deny and expired requests.
7. Toggle `/friend toggle` and confirm incoming requests are rejected.
8. Add/remove/list ignores.
9. Toggle notifications and check join/leave messages.
10. Use `/friend list 2` and GUI page buttons with more than 45 friends.
11. Restart the server and confirm saved friendships persist.
12. Test `/friendadmin reload` and permissions.

## Permission nodes
- `skyfriends.use` — default true
- `skyfriends.admin` — default op
- `skyfriends.bypass.limit` — default op
