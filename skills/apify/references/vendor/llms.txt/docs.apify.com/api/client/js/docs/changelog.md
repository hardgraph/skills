# Changelog

### [2.24.0](https://github.com/apify/apify-client-js/releases/tag/v2.24.0)[](#2240)

##### [2.24.0](https://github.com/apify/apify-client-js/releases/tag/v2.24.0) (2026-07-30)[](#2240-2026-07-30)

###### 🚀 Features[](#-features)

* Add `validateInput` method to `ActorClient` ([#957](https://github.com/apify/apify-client-js/pull/957)) ([3637586](https://github.com/apify/apify-client-js/commit/3637586f5a8534e86e1efc3b00711c7663463c93)) by [@Pijukatel](https://github.com/Pijukatel)
* Compress requests using brotli algo ([#962](https://github.com/apify/apify-client-js/pull/962)) ([6827899](https://github.com/apify/apify-client-js/commit/68278995ccdb272f1d8bf9c3627ecf2a91d4f386)) by [@mixalturek](https://github.com/mixalturek)

###### 🐛 Bug Fixes[](#-bug-fixes)

* ActorPermissionLevel was missing in ActorCollectionCreateOptions ([#940](https://github.com/apify/apify-client-js/pull/940)) ([4d8125e](https://github.com/apify/apify-client-js/commit/4d8125ebc1c6897d5cc2d1e602ef2d52468f2623)) by [@valekjo](https://github.com/valekjo)
* Fix incomplete type hint of last run/task ([#952](https://github.com/apify/apify-client-js/pull/952)) ([3924ce0](https://github.com/apify/apify-client-js/commit/3924ce0098e657c34c10dd55dca4ffd2b44c2bea)) by [@Pijukatel](https://github.com/Pijukatel)
* Align User-Agent OS token with other Apify clients ([#964](https://github.com/apify/apify-client-js/pull/964)) ([1f0e814](https://github.com/apify/apify-client-js/commit/1f0e81477d2898145d318515c452ccdd4cd1d760)) by [@Pijukatel](https://github.com/Pijukatel)

### [2.23.4](https://github.com/apify/apify-client-js/releases/tag/v2.23.4)[](#2234)

##### [2.23.4](https://github.com/apify/apify-client-js/releases/tag/v2.23.4) (2026-06-04)[](#2234-2026-06-04)

###### 🐛 Bug Fixes[](#-bug-fixes-1)

* Accept readonly arrays for webhook `eventTypes` input ([#931](https://github.com/apify/apify-client-js/pull/931)) ([1737867](https://github.com/apify/apify-client-js/commit/1737867817b8e70525a2d6c2ef1bfc4ffb1f8955)) by [@B4nan](https://github.com/B4nan)

### [2.23.3](https://github.com/apify/apify-client-js/releases/tag/v2.23.3)[](#2233)

##### [2.23.3](https://github.com/apify/apify-client-js/releases/tag/v2.23.3) (2026-05-11)[](#2233-2026-05-11)

###### 🐛 Bug Fixes[](#-bug-fixes-2)

* Drop only-allow preinstall, enforce pnpm via devEngines ([#895](https://github.com/apify/apify-client-js/pull/895)) ([27f7b7b](https://github.com/apify/apify-client-js/commit/27f7b7bed713eb4fd0585ef366d3022b37c82de9)) by [@B4nan](https://github.com/B4nan)

### [2.23.2](https://github.com/apify/apify-client-js/releases/tag/v2.23.2)[](#2232)

##### [2.23.2](https://github.com/apify/apify-client-js/releases/tag/v2.23.2) (2026-05-07)[](#2232-2026-05-07)

###### 🐛 Bug Fixes[](#-bug-fixes-3)

* Add support for new list request-queue requests parameters ([#880](https://github.com/apify/apify-client-js/pull/880)) ([c5f441b](https://github.com/apify/apify-client-js/commit/c5f441b50d52d528897cadfd53cbae9650366daa)) by [@mvolfik](https://github.com/mvolfik)

### [2.23.1](https://github.com/apify/apify-client-js/releases/tag/v2.23.1)[](#2231)

##### [2.23.1](https://github.com/apify/apify-client-js/releases/tag/v2.23.1) (2026-04-29)[](#2231-2026-04-29)

###### 🐛 Bug Fixes[](#-bug-fixes-4)

* Make Node-only imports bundler-discoverable ([#885](https://github.com/apify/apify-client-js/pull/885)) ([d57c048](https://github.com/apify/apify-client-js/commit/d57c048fed430e07a8a4fe48c3f3e5bbd3bebc19)) by [@barjin](https://github.com/barjin)

### [2.23.0](https://github.com/apify/apify-client-js/releases/tag/v2.23.0)[](#2230)

##### [2.23.0](https://github.com/apify/apify-client-js/releases/tag/v2.23.0) (2026-04-14)[](#2230-2026-04-14)

###### 🚀 Features[](#-features-1)

* Add output schema to ActorDefinition and includeUnrunnableActors to store API ([#881](https://github.com/apify/apify-client-js/pull/881)) ([c965d15](https://github.com/apify/apify-client-js/commit/c965d154a5e8f6e52e84761e566292d67a2e05d7)) by [@jancurn](https://github.com/jancurn)

###### 🐛 Bug Fixes[](#-bug-fixes-5)

* Turn any log streaming related errors to warnings ([#876](https://github.com/apify/apify-client-js/pull/876)) ([a3268ee](https://github.com/apify/apify-client-js/commit/a3268eeee38f1d47dbfa3fe0e0cfe26a949ca10d)) by [@Pijukatel](https://github.com/Pijukatel)

### [2.22.3](https://github.com/apify/apify-client-js/releases/tag/v2.22.3)[](#2223)

##### [2.22.3](https://github.com/apify/apify-client-js/releases/tag/v2.22.3) (2026-03-20)[](#2223-2026-03-20)

###### 🚀 Features[](#-features-2)

* Add the `ActorRunStorageIds` field to Actor Run responses ([#868](https://github.com/apify/apify-client-js/pull/868)) ([de8d315](https://github.com/apify/apify-client-js/commit/de8d3152e27f623ffd94fe9fa62962a7ab653a41)) by [@Pijukatel](https://github.com/Pijukatel)

###### 🐛 Bug Fixes[](#-bug-fixes-6)

* Use production mode and globalThis in browser bundle ([#870](https://github.com/apify/apify-client-js/pull/870)) ([b29c846](https://github.com/apify/apify-client-js/commit/b29c8465ef9a4726b596195b241e9a3649d832e1)) by [@MartinKristof](https://github.com/MartinKristof)

### [2.22.2](https://github.com/apify/apify-client-js/releases/tag/v2.22.2)[](#2222)

##### [2.22.2](https://github.com/apify/apify-client-js/releases/tag/v2.22.2) (2026-02-20)[](#2222-2026-02-20)

###### 🐛 Bug Fixes[](#-bug-fixes-7)

* Make all node-specific imports `async`, ignore in browser bundle ([#847](https://github.com/apify/apify-client-js/pull/847)) ([f61b417](https://github.com/apify/apify-client-js/commit/f61b417945e18eb8954af1ea1c28cb79ffb4558b)) by [@barjin](https://github.com/barjin)

### [2.22.1](https://github.com/apify/apify-client-js/releases/tag/v2.22.1)[](#2221)

##### [2.22.1](https://github.com/apify/apify-client-js/releases/tag/v2.22.1) (2026-02-13)[](#2221-2026-02-13)

###### 🚀 Features[](#-features-3)

* Add the new 'ownership' parameter to the storage list methods ([#835](https://github.com/apify/apify-client-js/pull/835)) ([7b328e1](https://github.com/apify/apify-client-js/commit/7b328e1eb1e5e90c717844fedcf4444027cd485e)) by [@nmanerikar](https://github.com/nmanerikar)
* Add `.&#x2F;browser` conditional export ([#845](https://github.com/apify/apify-client-js/pull/845)) ([fab3913](https://github.com/apify/apify-client-js/commit/fab3913c867c91315e8c0a92799e596b408874a1)) by [@barjin](https://github.com/barjin)
* Add the `readmeSummary` field to Actor responses ([#846](https://github.com/apify/apify-client-js/pull/846)) ([7d6bb2f](https://github.com/apify/apify-client-js/commit/7d6bb2f56db3702400efdf927106c817ad08310d)) by [@janbuchar](https://github.com/janbuchar)

### [2.22.0](https://github.com/apify/apify-client-js/releases/tag/v2.22.0)[](#2220)

##### [2.22.0](https://github.com/apify/apify-client-js/releases/tag/v2.22.0) (2026-01-27)[](#2220-2026-01-27)

###### 🚀 Features[](#-features-4)

* **actor:** Add 'taggedBuilds' to ActorUpdateOptions ([#830](https://github.com/apify/apify-client-js/pull/830)) ([c2c3560](https://github.com/apify/apify-client-js/commit/c2c35603d44678dcc0cdbf00897c9d4202a5fc67)) by [@l2ysho](https://github.com/l2ysho)
* **actor:** Updates ActorStandby type ([#833](https://github.com/apify/apify-client-js/pull/833)) ([10144e8](https://github.com/apify/apify-client-js/commit/10144e868eb484d096ace170c3282b4b8ba2f8be)) by [@l2ysho](https://github.com/l2ysho)

###### 🐛 Bug Fixes[](#-bug-fixes-8)

* Align `LimitsUpdateOptions` type with backend validation logic ([#820](https://github.com/apify/apify-client-js/pull/820)) ([74c1e2a](https://github.com/apify/apify-client-js/commit/74c1e2a7b91b976e1298084f6b6858f965c73fdd)) by [@barjin](https://github.com/barjin)
* Fetch key-value store records as attachments ([#821](https://github.com/apify/apify-client-js/pull/821)) ([2073e69](https://github.com/apify/apify-client-js/commit/2073e69a03beec9d3810797334479e3e5951766a)) by [@mvolfik](https://github.com/mvolfik)

###### ⚡ Performance[](#-performance)

* Omit `proxy-agent` from the browser bundle ([#816](https://github.com/apify/apify-client-js/pull/816)) ([d948b11](https://github.com/apify/apify-client-js/commit/d948b11e8d65f0d72b98fdbfe64d33eea0253a85)) by [@barjin](https://github.com/barjin)

### [2.21.0](https://github.com/apify/apify-client-js/releases/tag/v2.21.0)[](#2210)

##### [2.21.0](https://github.com/apify/apify-client-js/releases/tag/v2.21.0) (2025-12-11)[](#2210-2025-12-11)

###### 🚀 Features[](#-features-5)

* Add CONNECT tunneling support for HTTP proxy in sandboxed environments ([#791](https://github.com/apify/apify-client-js/pull/791)) ([a27d55a](https://github.com/apify/apify-client-js/commit/a27d55a504cdddebb5a4afa594a94784d27d3dba)) by [@tducret](https://github.com/tducret)
* Make all `collectionClient.list` methods return value also be `asyncIterator` of relevant data ([#790](https://github.com/apify/apify-client-js/pull/790)) ([f855fd4](https://github.com/apify/apify-client-js/commit/f855fd4a774ee8fe91671711f530203475ee1dbd)) by [@Pijukatel](https://github.com/Pijukatel)
* Generated JSDocs based on the API reference ([#797](https://github.com/apify/apify-client-js/pull/797)) ([85653a1](https://github.com/apify/apify-client-js/commit/85653a171fcb699d6e8eb16caa481980ecc50ae8)) by [@jancurn](https://github.com/jancurn)
* Make storage clients list methods return value also be asyncIterator of relevant data ([#803](https://github.com/apify/apify-client-js/pull/803)) ([c58ce6f](https://github.com/apify/apify-client-js/commit/c58ce6f11363e1863b53754427aed93a185f4ae1)) by [@Pijukatel](https://github.com/Pijukatel)
* Expose `actorPermissionLevel` in Actor client ([#809](https://github.com/apify/apify-client-js/pull/809)) ([513e41c](https://github.com/apify/apify-client-js/commit/513e41ce16bb7a1269513337ee807ff4e3664f47)) by [@stepskop](https://github.com/stepskop)

### [2.20.0](https://github.com/apify/apify-client-js/releases/tag/v2.20.0)[](#2200)

##### [2.20.0](https://github.com/apify/apify-client-js/releases/tag/v2.20.0) (2025-11-20)[](#2200-2025-11-20)

###### 🚀 Features[](#-features-6)

* Add redirected actor logs ([#769](https://github.com/apify/apify-client-js/pull/769)) ([a7f4233](https://github.com/apify/apify-client-js/commit/a7f42333796b294266dd7950a2ecf47fa504373c)) by [@Pijukatel](https://github.com/Pijukatel)
* Replace `agentkeepalive` with native Node.js HTTP agents for proxy support ([#788](https://github.com/apify/apify-client-js/pull/788)) ([7d2be0f](https://github.com/apify/apify-client-js/commit/7d2be0f832a9cba2f7ada14fa80f66e6ea50b944)) by [@tducret](https://github.com/tducret)

###### 🐛 Bug Fixes[](#-bug-fixes-9)

* Actor start and run options and doc ([#785](https://github.com/apify/apify-client-js/pull/785)) ([61f91e5](https://github.com/apify/apify-client-js/commit/61f91e5d2bad1c622a40a11d0d321443e68c4540)) by [@michael-apify](https://github.com/michael-apify)

###### ⚡ Performance[](#-performance-1)

* Dynamic imports for certain `node:` packages ([#767](https://github.com/apify/apify-client-js/pull/767)) ([0bf8db7](https://github.com/apify/apify-client-js/commit/0bf8db7ce61c98232d7051bf1f74e70fb89df8a5)) by [@barjin](https://github.com/barjin)
* Don't bundle `node:crypto` in the browser bundle ([#782](https://github.com/apify/apify-client-js/pull/782)) ([72a1d3c](https://github.com/apify/apify-client-js/commit/72a1d3c154b1db1e3b6f89b69a0593e44bc5e062)) by [@barjin](https://github.com/barjin)

### [2.19.0](https://github.com/apify/apify-client-js/releases/tag/v2.19.0)[](#2190)

##### [2.19.0](https://github.com/apify/apify-client-js/releases/tag/v2.19.0) (2025-10-20)[](#2190-2025-10-20)

###### 🚀 Features[](#-features-7)

* Move restartOnError from Actor to Run options ([#760](https://github.com/apify/apify-client-js/pull/760)) ([8f80f82](https://github.com/apify/apify-client-js/commit/8f80f82c22128fd3378ba00ad29766cf4cc8e3c0)) by [@DaveHanns](https://github.com/DaveHanns)

### [2.18.0](https://github.com/apify/apify-client-js/releases/tag/v2.18.0)[](#2180)

##### [2.18.0](https://github.com/apify/apify-client-js/releases/tag/v2.18.0) (2025-10-09)[](#2180-2025-10-09)

###### 🚀 Features[](#-features-8)

* Allowed signature to be passed in kv-store/datasets ([#761](https://github.com/apify/apify-client-js/pull/761)) ([a31e36d](https://github.com/apify/apify-client-js/commit/a31e36d6201f90136da362af2aa10b29efb80bad)) by [@gippy](https://github.com/gippy)
* Add startedBefore and startedAfter to run list ([#763](https://github.com/apify/apify-client-js/pull/763)) ([2345999](https://github.com/apify/apify-client-js/commit/23459990598ba01833a21bfe969a1c64f775be00)) by [@danpoletaev](https://github.com/danpoletaev)

###### 🐛 Bug Fixes[](#-bug-fixes-10)

* Export missing symbols from env vars and version client ([#756](https://github.com/apify/apify-client-js/pull/756)) ([86b591f](https://github.com/apify/apify-client-js/commit/86b591fe8d2f07b4e746561ee9e055fca6639e1d)) by [@B4nan](https://github.com/B4nan)

### [2.17.0](https://github.com/apify/apify-client-js/releases/tag/v2.17.0)[](#2170)

##### [2.17.0](https://github.com/apify/apify-client-js/releases/tag/v2.17.0) (2025-09-11)[](#2170-2025-09-11)

###### 🚀 Features[](#-features-9)

* Add forcePermissionLevel run option ([#743](https://github.com/apify/apify-client-js/pull/743)) ([693808c](https://github.com/apify/apify-client-js/commit/693808c6dbbf24542f8f86f3d49673b75309e9f6)) by [@tobice](https://github.com/tobice)

###### 🐛 Bug Fixes[](#-bug-fixes-11)

* Signed storage URLs avoid adding expiresInSecs to query params ([#734](https://github.com/apify/apify-client-js/pull/734)) ([70aff4f](https://github.com/apify/apify-client-js/commit/70aff4fedefc02a1c8c6e5155057e213a8ad6c81)) by [@danpoletaev](https://github.com/danpoletaev)
* Presigned resource urls shouldn't follow `baseUrl` ([#745](https://github.com/apify/apify-client-js/pull/745)) ([07b36fb](https://github.com/apify/apify-client-js/commit/07b36fbd46ed74e9c4ad3977cac883af55ad525d)) by [@barjin](https://github.com/barjin)

### [2.16.0](https://github.com/apify/apify-client-js/releases/tag/v2.16.0)[](#2160)

##### [2.16.0](https://github.com/apify/apify-client-js/releases/tag/v2.16.0) (2025-08-26)[](#2160-2025-08-26)

###### Refactor[](#refactor)

* \[**breaking**] Rename expiresInMillis to expiresInSecs in create storage content URL ([#733](https://github.com/apify/apify-client-js/pull/733)) ([a190b72](https://github.com/apify/apify-client-js/commit/a190b72f6f62ffb54898fd74c80981a6967d573f)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.15.1](https://github.com/apify/apify-client-js/releases/tag/v2.15.1)[](#2151)

##### [2.15.1](https://github.com/apify/apify-client-js/releases/tag/v2.15.1) (2025-08-20)[](#2151-2025-08-20)

###### 🐛 Bug Fixes[](#-bug-fixes-12)

* Add recordPublicUrl to KeyValueListItem type ([#730](https://github.com/apify/apify-client-js/pull/730)) ([42dfe64](https://github.com/apify/apify-client-js/commit/42dfe6484e3504aaf46c516bade3d7ff989782ea)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.15.0](https://github.com/apify/apify-client-js/releases/tag/v2.15.0)[](#2150)

##### [2.15.0](https://github.com/apify/apify-client-js/releases/tag/v2.15.0) (2025-08-12)[](#2150-2025-08-12)

###### 🚀 Features[](#-features-10)

* Extend status parameter to an array of possible statuses ([#723](https://github.com/apify/apify-client-js/pull/723)) ([0be893f](https://github.com/apify/apify-client-js/commit/0be893f2401a652908aff1ed305736068ee0b421)) by [@JanHranicky](https://github.com/JanHranicky)

### [2.14.0](https://github.com/apify/apify-client-js/releases/tag/v2.14.0)[](#2140)

##### [2.14.0](https://github.com/apify/apify-client-js/releases/tag/v2.14.0) (2025-08-11)[](#2140-2025-08-11)

###### 🚀 Features[](#-features-11)

* Add keyValueStore.getRecordPublicUrl ([#725](https://github.com/apify/apify-client-js/pull/725)) ([d84a03a](https://github.com/apify/apify-client-js/commit/d84a03afe6fd49e38d4ca9a6821681e852c73a2a)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.13.0](https://github.com/apify/apify-client-js/releases/tag/v2.13.0)[](#2130)

##### [2.13.0](https://github.com/apify/apify-client-js/releases/tag/v2.13.0) (2025-08-06)[](#2130-2025-08-06)

###### 🚀 Features[](#-features-12)

* Add new methods Dataset.createItemsPublicUrl & KeyValueStore.createKeysPublicUrl ([#720](https://github.com/apify/apify-client-js/pull/720)) ([62554e4](https://github.com/apify/apify-client-js/commit/62554e48a8bf6bf1853f356ac84f046fed5945c1)) by [@danpoletaev](https://github.com/danpoletaev)

###### 🐛 Bug Fixes[](#-bug-fixes-13)

* Add `eventData` to `WebhookDispatch` type ([#714](https://github.com/apify/apify-client-js/pull/714)) ([351f11f](https://github.com/apify/apify-client-js/commit/351f11f268a54532c7003ab099bc0d7d8d9c9ad7)) by [@valekjo](https://github.com/valekjo)
* KV store createKeysPublicUrl wrong URL ([#724](https://github.com/apify/apify-client-js/pull/724)) ([a48ec58](https://github.com/apify/apify-client-js/commit/a48ec58e16a36cc8aa188524e4a738c40f5b74e9)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.12.6](https://github.com/apify/apify-client-js/releases/tag/v2.12.6)[](#2126)

##### [2.12.6](https://github.com/apify/apify-client-js/releases/tag/v2.12.6) (2025-06-30)[](#2126-2025-06-30)

###### 🚀 Features[](#-features-13)

* Allow sorting of Actors collection ([#708](https://github.com/apify/apify-client-js/pull/708)) ([562a193](https://github.com/apify/apify-client-js/commit/562a193b90ce4f2b05bf166da8fe2dddaa87eb6b)) by [@protoss70](https://github.com/protoss70)

###### 🐛 Bug Fixes[](#-bug-fixes-14)

* Use appropriate timeouts ([#704](https://github.com/apify/apify-client-js/pull/704)) ([b896bf2](https://github.com/apify/apify-client-js/commit/b896bf2e653e0766ef297f29a35304c1a5f27598)) by [@janbuchar](https://github.com/janbuchar)
* Rename option for new sortBy parameter ([#711](https://github.com/apify/apify-client-js/pull/711)) ([f45dd03](https://github.com/apify/apify-client-js/commit/f45dd037c581a6c0e27fd8c036033b99cec1ba89)) by [@protoss70](https://github.com/protoss70)

### [2.12.5](https://github.com/apify/apify-client-js/releases/tag/v2.12.5)[](#2125)

##### [2.12.5](https://github.com/apify/apify-client-js/releases/tag/v2.12.5) (2025-05-28)[](#2125-2025-05-28)

###### 🚀 Features[](#-features-14)

* List kv store keys by collection of prefix ([#688](https://github.com/apify/apify-client-js/pull/688)) ([be25137](https://github.com/apify/apify-client-js/commit/be25137575435547aaf2c3849fc772daf0537450)) by [@MFori](https://github.com/MFori)
* Add unlockRequests endpoint to RequestQueue client ([#700](https://github.com/apify/apify-client-js/pull/700)) ([7c52c64](https://github.com/apify/apify-client-js/commit/7c52c645e2eb66ad97c8daa9791b080bfc747288)) by [@drobnikj](https://github.com/drobnikj)

###### 🐛 Bug Fixes[](#-bug-fixes-15)

* Add missing 'effectivePlatformFeatures', 'createdAt', 'isPaying' to User interface ([#691](https://github.com/apify/apify-client-js/pull/691)) ([e138093](https://github.com/apify/apify-client-js/commit/e1380933476e5336469e5da083d2017147518f88)) by [@metalwarrior665](https://github.com/metalwarrior665)
* Move prettier into `devDependencies` ([#695](https://github.com/apify/apify-client-js/pull/695)) ([1ba903a](https://github.com/apify/apify-client-js/commit/1ba903a1bfa7a95a8c54ef53951db502dfa4b276)) by [@hudson-worden](https://github.com/hudson-worden)

### [2.12.4](https://github.com/apify/apify-client-js/releases/tag/v2.12.4)[](#2124)

##### [2.12.4](https://github.com/apify/apify-client-js/releases/tag/v2.12.4) (2025-05-13)[](#2124-2025-05-13)

###### 🚀 Features[](#-features-15)

* Allow overriding timeout of `KVS.setRecord` calls ([#692](https://github.com/apify/apify-client-js/pull/692)) ([105bd68](https://github.com/apify/apify-client-js/commit/105bd6888117a6c64b21a725c536d4992dff099c)) by [@B4nan](https://github.com/B4nan)

###### 🐛 Bug Fixes[](#-bug-fixes-16)

* Fix `RunCollectionListOptions` status type ([#681](https://github.com/apify/apify-client-js/pull/681)) ([8fbcf82](https://github.com/apify/apify-client-js/commit/8fbcf82bfaca57d087719cf079fc850c6d31daa5)) by [@MatousMarik](https://github.com/MatousMarik)
* **actor:** Add missing 'pricingInfos' field to Actor object ([#683](https://github.com/apify/apify-client-js/pull/683)) ([4bd4853](https://github.com/apify/apify-client-js/commit/4bd485369ac42d0b72597638c0316a6ca60f9847)) by [@metalwarrior665](https://github.com/metalwarrior665)

### [2.12.3](https://github.com/apify/apify-client-js/releases/tag/v2.12.3)[](#2123)

##### [2.12.3](https://github.com/apify/apify-client-js/releases/tag/v2.12.3) (2025-04-24)[](#2123-2025-04-24)

###### 🐛 Bug Fixes[](#-bug-fixes-17)

* DefaultBuild() returns BuildClient ([#677](https://github.com/apify/apify-client-js/pull/677)) ([8ce72a4](https://github.com/apify/apify-client-js/commit/8ce72a4c90aac421281d14ad0ff25fdecba1d094)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.12.2](https://github.com/apify/apify-client-js/releases/tag/v2.12.2)[](#2122)

##### [2.12.2](https://github.com/apify/apify-client-js/releases/tag/v2.12.2) (2025-04-14)[](#2122-2025-04-14)

###### 🚀 Features[](#-features-16)

* Add support for general resource access ([#669](https://github.com/apify/apify-client-js/pull/669)) ([7deba52](https://github.com/apify/apify-client-js/commit/7deba52a5ff96c990254687d6b965fc1a5bf3467)) by [@tobice](https://github.com/tobice)
* Add defaultBuild method ([#668](https://github.com/apify/apify-client-js/pull/668)) ([c494b3b](https://github.com/apify/apify-client-js/commit/c494b3b8b664a88620e9f41c902acba533d636cf)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.12.1](https://github.com/apify/apify-client-js/releases/tag/v2.12.1)[](#2121)

##### [2.12.1](https://github.com/apify/apify-client-js/releases/tag/v2.12.1) (2025-03-11)[](#2121-2025-03-11)

###### 🚀 Features[](#-features-17)

* Add maxItems and maxTotalChargeUsd to resurrect ([#652](https://github.com/apify/apify-client-js/pull/652)) ([5fb9c9a](https://github.com/apify/apify-client-js/commit/5fb9c9a35d6ccb7313c5cbbd7d09b19a64d70d8e)) by [@novotnyj](https://github.com/novotnyj)

### [2.11.2](https://github.com/apify/apify-client-js/releases/tag/v2.11.2)[](#2112)

##### [2.11.2](https://github.com/apify/apify-client-js/releases/tag/v2.11.2) (2025-02-03)[](#2112-2025-02-03)

###### 🚀 Features[](#-features-18)

* Add dataset.statistics ([#621](https://github.com/apify/apify-client-js/pull/621)) ([6aeb2b7](https://github.com/apify/apify-client-js/commit/6aeb2b7fae041468d125a0c8bbb00804e290143a)) by [@MFori](https://github.com/MFori)
* Added getOpenApiSpecification() to BuildClient ([#626](https://github.com/apify/apify-client-js/pull/626)) ([6248b28](https://github.com/apify/apify-client-js/commit/6248b2844796f93e22404ddea85ee77c1a5b7d50)) by [@danpoletaev](https://github.com/danpoletaev)

### [2.11.1](https://github.com/apify/apify-client-js/releases/tag/v2.11.1)[](#2111)

##### [2.11.1](https://github.com/apify/apify-client-js/releases/tag/v2.11.1) (2025-01-10)[](#2111-2025-01-10)

###### 🐛 Bug Fixes[](#-bug-fixes-18)

* Change type `Build.actorDefinitions` to `Build.actorDefinition` ([#624](https://github.com/apify/apify-client-js/pull/624)) ([611f313](https://github.com/apify/apify-client-js/commit/611f31365727e70f58d899009ff5a05c6b888253)) by [@jirispilka](https://github.com/jirispilka)
* Add ActorRunPricingInfo type ([#623](https://github.com/apify/apify-client-js/pull/623)) ([8880295](https://github.com/apify/apify-client-js/commit/8880295f13c1664ab6ae0b8b3f171025317ea011)) by [@janbuchar](https://github.com/janbuchar)

### [2.11.0](https://github.com/apify/apify-client-js/releases/tag/v2.11.0)[](#2110)

##### [2.11.0](https://github.com/apify/apify-client-js/releases/tag/v2.11.0) (2024-12-16)[](#2110-2024-12-16)

###### 🚀 Features[](#-features-19)

* **actor-build:** Add actorDefinition type for actor build detail, deprecate inputSchema and readme. ([#611](https://github.com/apify/apify-client-js/pull/611)) ([123c2b8](https://github.com/apify/apify-client-js/commit/123c2b81c945a0ca6922221598aa73c42cc298d6)) by [@drobnikj](https://github.com/drobnikj)
* Add `charge` method to the run client for "pay per event" ([#613](https://github.com/apify/apify-client-js/pull/613)) ([3d9c64d](https://github.com/apify/apify-client-js/commit/3d9c64d5442b4f8f27c2b19dd98dd3b758944287)) by [@Jkuzz](https://github.com/Jkuzz)
* **request-queue:** Add queueHasLockedRequests and clientKey into RequestQueueClientListAndLockHeadResult ([#617](https://github.com/apify/apify-client-js/pull/617)) ([f58ce98](https://github.com/apify/apify-client-js/commit/f58ce989e431de54eb673e561e407a7066ea2b64)) by [@drobnikj](https://github.com/drobnikj)

###### 🐛 Bug Fixes[](#-bug-fixes-19)

* **actor:** Correctly set type for ActorTaggedBuilds ([#612](https://github.com/apify/apify-client-js/pull/612)) ([3bda7ee](https://github.com/apify/apify-client-js/commit/3bda7ee741caf2ccfea249a42ed7512cda36bf0b)) by [@metalwarrior665](https://github.com/metalwarrior665)

### [2.10.0](https://github.com/apify/apify-client-js/releases/tag/v2.10.0)[](#2100)

##### [2.10.0](https://github.com/apify/apify-client-js/releases/tag/v2.10.0) (2024-11-01)[](#2100-2024-11-01)

###### 🚀 Features[](#-features-20)

* Add user.updateLimits ([#595](https://github.com/apify/apify-client-js/pull/595)) ([bf97c0f](https://github.com/apify/apify-client-js/commit/bf97c0f5bf8d0cbd8decb60382f0605243b00dd5)) by [@MFori](https://github.com/MFori)
* Allow appending custom parts to the user agent ([#602](https://github.com/apify/apify-client-js/pull/602)) ([d07452b](https://github.com/apify/apify-client-js/commit/d07452b7bff83d16b48bf3cfba5b88aa564ffe2b)) by [@B4nan](https://github.com/B4nan)

###### 🐛 Bug Fixes[](#-bug-fixes-20)

* Allow `null` when updating dataset/kvs/rq `name` ([#604](https://github.com/apify/apify-client-js/pull/604)) ([0034c2e](https://github.com/apify/apify-client-js/commit/0034c2ee63d6d1c6856c4e7786da43d86a3d63ce)) by [@B4nan](https://github.com/B4nan)
