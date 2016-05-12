# Gossip: Vicinity in Javascript

[![Build Status](https://travis-ci.org/nicola/js-gossip-vicinity.svg?style=flat-square)](https://travis-ci.org/nicola/js-gossip-vicinity)

This implements the Vicinity gossip protocol [1] for membership management.

## Vicinity

```js
const VicinityPeer = require('gossip-vicinity')
const parallel = require('run-parallel')

const Alice = new VicinityPeer()
const Bob = new VicinityPeer()

Alice.addPeers([Bob.me])

parallel([
  () => Alice.listen()
  () => Bob.listen()
], (err) => {
  if (!err) {
    Alice.start()
    Bob.start()
    // This will make Alice and Bob exchange each others information every 1 second
  }
})

```

#### var peer = new VicinityPeer(opts)

`opts` can have:
- `peer`: [PeerInfo](http://npm.im/peer-info) object of this VicinityPeer, default: `new PeerInfo()`.
- `maxShuffle`: how many peers to send during shuffling
- `maxPeers`: how many peers can be stored in the list of neighboors (or `.partialView`)
- `interval`: how often VicinityPeer should shuffle
- `peers`: array of peers to bootstrap VicinityPeer (they will be added to its `.partialView`)

#### peer.listen()

Listen on its transports (by default TCP)

#### peer.close()

Close any listener on any transport

#### peer.start()

Start shuffling every `peer.interval`

#### peer.stop()

Stops the repeating shuffling

#### peer.shuffle(cb)

Shuffle and when done calls `cb` (on failure or success)

#### peer.addPeers(peers, replace)

Add a list of peers, if the peers to be added will make `.partialView` grow beyond `.maxPeers`, the `replace` list will be used, otherwise drop 'em.

#### peer.updateAge()

Update the age of the peers in the `.partialView`

## References

[1] S. Voulgaris, M. van Steen. [Epidemic-Style Management of Semantic Overlays for Content-Based Searching](http://www.distributed-systems.net/papers/2005.europar.pdf). Euro Par (2015)

## License

MIT