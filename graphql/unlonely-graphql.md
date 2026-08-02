# Unlonely GraphQL API

## Overview

Unlonely was a crypto/web3 livestreaming platform — livestreams on Base with token-gated chat, creator tokens, on-chain viewer interactions, and stream clips minted as "NFCs" (non-fungible clips). Its backend was a single GraphQL API served by the open-source Node/TypeScript + Prisma server in the `unlonely-alpha` monorepo.

This is a REAL schema, not a conceptual one: `unlonely-schema.json` is the introspection document saved verbatim from the open-source server repository, and `unlonely-schema.graphql` is an SDL rendering of that same introspection.

- **Introspection source (verbatim):** https://raw.githubusercontent.com/bguan2020/unlonely-alpha/master/unlonely-alpha/server/schema.json
- **Monorepo:** https://github.com/bguan2020/unlonely-alpha (web + mobile + server + Solidity contracts)

> **Status: defunct.** The unlonely.app domain registration has expired (Namecheap parking page as of 2026-07) and the hosted GraphQL endpoint is no longer live. This artifact preserves the API surface the platform served. See `lifecycle/unlonely-lifecycle.yml`.

## Provider

- **Name:** Unlonely
- **Founders:** Grace Guan and Brian Guan
- **Backing:** Multicoin Capital (portfolio source for this profile)
- **Website (expired):** https://unlonely.app
- **X:** https://x.com/unlonely_app

## Domain Coverage

The server's Prisma entity modules (each contributing typeDefs + resolvers) cover:

- **Channel / Room / StreamInteraction** — livestream channels, their rooms, and viewer interactions with a live stream
- **Chat / ChatCommand / Like** — token-gated stream chat, chat commands, and reactions
- **NFC** — "non-fungible clips": stream clips saved and minted by viewers
- **CreatorToken / TempToken / Vibes** — creator tokens, ephemeral "temp tokens", and the $VIBES token economy powering on-chain viewer interaction
- **GamblableInteraction / BaseLeaderboard** — bet/vote-style on-chain interactions and leaderboards on Base
- **User / DeviceToken / Subscription / Package / POAP / Task / Video** — accounts (wallet-based), push tokens, subscriptions, packages, POAPs, tasks, and videos

## Operation Surface

The schema exposes 56 root queries and 66 root mutations across 41 object types. Representative operations (all names verbatim from the introspection):

| Area | Queries | Mutations |
|---|---|---|
| Channels | `getChannelBySlug`, `getChannelFeed`, `getChannelSearchResults`, `getChannelsByOwnerAddress` | `postChannel`, `migrateChannelToLivepeer`, `updateChannelText`, `updateChannelVibesTokenPriceRange`, `softDeleteChannel` |
| Chat | `getRecentChats`, `firstChatExists`, `chatBot` | `postFirstChat`, `postChatByAwsId`, `updatePinnedChatMessages`, `updateDeleteChatCommands` |
| NFCs (clips) | `getNFC`, `getNFCFeed`, `getNFCByTokenData`, `getLivepeerClipData` | `createClip`, `createLivepeerClip`, `postNFC`, `updateNFC`, `openseaNFCScript` |
| Tokens | `getTempTokens`, `getTokenLeaderboard`, `getTokenHoldersByChannel`, `getVibesTransactions` | `createCreatorToken`, `postTempToken`, `postVibesTrades`, `updateCreatorTokenPrice`, `updateTempTokenIsAlwaysTradeable` |
| Betting / events | `getBetsByChannel`, `getGamblableEventLeaderboardByChannelId`, `getUnclaimedEvents`, `getBaseLeaderboard` | `postBet`, `postBetTrade`, `postSharesEvent`, `postClaimPayout`, `closeSharesEvents` |
| Streaming (Livepeer) | `getLivepeerStreamData`, `getLivepeerStreamSessionsData`, `getLivepeerViewershipMetrics` | `updateLivepeerStreamData`, `requestUploadFromLivepeer`, `trimVideo`, `concatenateOutroToTrimmedVideo` |
| Users / notifications | `getUser`, `currentUser`, `currentUserAuthMessage`, `getAllUsersWithNotificationsToken` | `updateUser`, `updateUsername`, `postDeviceToken`, `postSubscription`, `updateUserNotifications` |

Auth was wallet-based: `currentUserAuthMessage` / `getDoesUserAddressMatch` support a signed-message (Sign-In-with-Ethereum-style) flow; the API had no published OAuth or API-key program.

## Schema Files

- `unlonely-schema.json` — verbatim GraphQL introspection (225 types) from the server repo
- `unlonely-schema.graphql` — SDL rendered from the introspection file
- `../data-model/unlonely-data-model.yml` — entity-relationship graph derived from this introspection
