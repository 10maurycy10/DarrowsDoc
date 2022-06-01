# Darrows API Documentation

## Initialization

Client sends join packet, server responds with init packet.

Client then sets name by sending "/name ${name}" in chat.

Client proceeds to send ping packets every 1000ms.

## Types

### InputState

```
up: bool,
right: bool,
left: bool,
down: bool,
arrowLeft: bool,
arrowRight: bool,
space: bool,
shift: bool
```

### Obstacle

```
x: Number, 
y: Number,
width: Number, 
height: Number, 
type: 'obstacle'/'bounce'/'point'
```

### Arrow

```
x: Number,
y: Number,
angle: Number -- radians
```

### Player

```
abilityCd: Number -- ability cooldowm
angle: Number -- the players angle, in radians
arrowing: Number/bool
characterName: String -- the name of the charater in use
dev: String -- true if the player is in developer mode. unobtanable otherwize
dying: bool -- is the player about to die?
maxCd: Number -- Cooldown after using abilty
name: String -- the players ign
passive: true -- Whether the player is in passive mode, including when invunrable
radius: Number -- size of player, used for death animation
score: Number -- the players score
timer: Number 
timerMax: Number
x: Number -- The player position
y: Number -- The player position
```

### Packs

The format for packs is

```
<Data> {
	id: String
	data: Data
}
```

Packs are sparse, encoding only modified entry's and have no way to remove an object.

## ClientBound (S2C)

### serverTickMs 

```
serverTickMs: the servers tick rate
```

The serverTickMs values encodes the time per server tick, used by the client for interpolation

### init

```
type: "init"
selfId: id
players: players
```

This packet is send when the player joins the game, encoding the players id and the initial player table.

### saveName

```
saveName: name
```

Sent the the client to indicate that it should save a player name, in the vanilla client, this is done with localstorage.

On later connections the client will send a chat packet with the name, "/name ${savedName}".

### teamMode

```
teamMode: bool
```

Sent by server when the game mode changes.

### arena

```
arena: {width, heigh}
```

Sets the map size

### obstacles

```
obstacles: Array<Obstacle>
```

Sends the obstacle list.

### blocks

```
blocks: Array<Blocks>
```

unused

### arrows

```
arrows: Array<Pack<Arrow>>
```

Send to update the arrow table, unused by vanilla server.

### version

```
version: String
```

sets the servers version number.

### stats

```
kills: Number
death: Number
kdr: Number
accuracy: Number
```

Displays the stats overlay.

### leader

```
leader: {
	name: String,
	id: String
}
```

encodes the current leader.

### speed

```
speed: Number
```

sets the players speed value.

### fric

```
fric: Number
```

sets the players fric value.

### chat

```
name: String
msg: String
dev: bool
```
Adds a chat message to the chat window.

### kill

```
kills: Number
hit: Player
```

Sent when the player kills another player

### arrowHit

```
arrowHit: true
```

sent when the player is hit.

### pung

```
pung: Number
```

sent echoing the value in ping packets, can be used to calculate round trip time.

### newPlayer

```
type: "newPlayer"
id: String
player: Player
```

Indicates a player has joined, sent with ``id == selfId`` when the player respawns

### leave

```
type: "leave"
id: String
```

Indicates that a user has left or died.

### round

```
round.intermission: bool
round.time: Number
```

Indicates the current round state

### arrowReset

```
arrowReset: true
```

clears all arrows, unused by vanilla server.

### state

```
d {
	opt spacing: [Number; 3],
	opt p: Array<Pack<Player>,
	opt a: Array<Pack<Arrow>>
}
```

Encodes the current game state

The arrow and player objects have unmodified property's omitted to save space.

## ServerBound (C2S)

### join

```
joinE: true
character: String
```

joins the game.

### chat

```
chat: String
```

sends a message in chat

### ping

```
ping: Number
```

current time in unix milis, echoed by server.

### input

```
data: InputState
input: true
```

Sends current inputs to server.
