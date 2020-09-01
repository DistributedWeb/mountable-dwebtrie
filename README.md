# mountable-dwebtrie
[![Build Status](https://travis-ci.com/andrewosh/mountable-dwebtrie.svg?token=WgJmQm3Kc6qzq1pzYrkx&branch=master)](https://travis-ci.com/andrewosh/mountable-dwebtrie)

A DWebtrie wrapper that supports mounting of sub-Hypertries.

### Usage
A MountableHypertrie can be mounted within another MountableHypertrie by using the `mount` command:
```js
const store = dwebstore(ram)
const trie1 = new MountableHypertrie(store)
const trie2 = new MountableHypertrie(store)

trie2.ready(() => {
  trie1.mount('/a', trie2.key, ...)
})
```
Assuming `trie2` has a value 'hello' at `/b/c`:
```js
trie1.get('/a/b/c', console.log) // Will return Buffer.from('hello')
```

A mount can be removed by performing a `del` on the mountpoint :
```js
trie1.del('/a', err => {
  trie1.get('/a/b/c', console.log) // Will print `null`
})
```
### API
`mountable-dwebtrie` re-exposes the [`dwebtrie`](https://github.com/distributedweb/dwebtrie) API, with the addition of the following methods (and a different constructor):

#### `const trie = new MountableHypertrie(dwebstore, key, opts)`
- `dwebstore`: any object that implements the dwebstore interface. For now, it's recommanded to use [`random-access-dwebstore`](https://github.com/andrewosh/random-access-dwebstore)
- `key` is the dwebtrie key
- `opts` can contain any `dwebtrie` options

#### `trie.mount(path, key, opts, cb)`
- `path` is the mountpoint
- `key` is the key for the MountableHypertrie to be mounted at `path`

`opts` can include:
```
{
  remotePath: '/remote/path', // An optional base path within the mount.
  version: 1                  // An optional checkout version
}
```

_Note: We're still adding support for many dwebtrie methods. Here's what's been implemented so far:_

- [x] `get`
- [x] `put`
- [x] `del`
- [ ] `batch`
- [x] `iterator`
- [x] `list`
- [x] `createReadStream`
- [ ] `createWriteStream`
- [ ] `checkout`
- [x] `watch`
- [ ] `createHistoryStream`
- [ ] `createDiffStream`

### License
MIT
