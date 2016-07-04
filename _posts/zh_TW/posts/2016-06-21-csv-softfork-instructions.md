---
type: posts
layout: post
lang: zh_TW
name: csv-softfork-instructions
id: en-csv-softfork-instructions
title: CSV 軟分叉 - 礦工升級重要指引
permalink: /zh_TW/2016/06/21/csv-softfork-instructions/
categories: [mining]
tags: [軟分叉, soft fork, soft forks, bip9, version bits, mining, bip68, bip112, bip113]
version: 1
excerpt: 比特幣的共識規則現正有一項軟分叉進行中，雖然一切看起來均運作正常，礦工和礦池負責人絕對不能忽視以下的重要資訊和工作清單。
---
{% include _toc.html %}

比特幣的共識規則現正有一項軟分叉進行中，雖然一切看起來均運作正常，礦工和礦池負責人絕對不能忽視以下的重要資訊和工作清單。

礦工和礦池負責人如有任何疑問，歡迎和我們[聯絡][1]。

摘要

1. 你必須在區塊419328號產出以前，檢查你的所有節點，確保皆已升級至 Bitcoin Core 0.12.1 或相容軟件。

2. 如果你正在手動設定區塊版本號，你必在區塊419328號產出以前把版本號的0號位元去除。最好的做法是由 bitcoind 自動設定。

3. Coinbase 交易的 nSequence 值請設定為 0xffffffff，以避免和 [BIP68][] 及 [BIP113][] 出現衝突。

4. 如果你必須使用其它 nSequence 值，你必須小心跟從以下指引。

## CSV 軟分叉情況

"CSV" 軟分叉已經達到「鎖定」的要求，將會正式啟動。在415296至417311號一共2016個區塊之中，1946個(96.53%)顯示已準備實施[BIP68][], [BIP112][] 和 [BIP113][] (“CSV”) 軟分叉。從417312號區塊(2016-06-21 05:18:58 UTC)開始，CSV 軟分叉進入為時約兩星期的「鎖定」寬限期，直至419327號區塊。之後，[BIP68][], [BIP112][] 和 [BIP113][]的新規則便會正式實施。該軟分叉已過了臨界點，如非有極大規模的區塊鏈回滾，其實施已不可逆轉。

## 所有礦工請留意

在寬限期內，所有礦工必須升級至 Bitcoin Core 0.12.1 或任何支持 CSV 軟分叉的相容軟件。實際上，在編寫本文的時間，Bitcoin Core 0.12.1 是唯一支持 CSV 軟分叉的版本。礦工必須再三確認所有負責挖礦的節點以及備用節點皆已升級。

不依從以上指引者可能會產出無效區塊，或會延長無效的區塊鏈，造成區塊鏈分叉，令有關礦工和廣大比特幣使用者蒙受經濟損失。

## 手動設定區塊版本號的礦工請注意

Bitcoin Core 的默認設定會自動按 BIP9 的要求設定區塊版本，然而我們留意到有些礦工會手動設定區塊版本號，我們強烈建議不要手動設定區塊版本號，因為這會對比特幣系統帶來風險。

如果有礦工設定了一個版本編號，卻沒有實施對應的規則，便可能會產出無效區塊，並會延長無效的區塊鏈。簡單而言，不使用由 bitcoind 提供的默認版本值，可能會令版本編號與實際運作的規則不相符。

和 BIP33/66/65 所使用的 IsSuperMajority 軟分叉不同，在 BIP9 的設計中，沒有區塊會因為不當版本號而被視作無效（根據 BIP65，最低有效版本號為4）。因此，礦工並無誘因去手動設定區塊版本號，因為這只會帶來不必要的維護工作與人為出錯的風險。

然而，如果你並不依從以上建議，仍然是手動設定區塊版本號，你必須作出以下行動。在「鎖定」寬限期內，你必須去除 CSV 的版本號位元，即0號位元。例如，如果你本來的版本號是 0x20000001，你就必須在419328號區塊前改變為 0x20000000，否則你將會令所有支持 BIP9 的節點的日誌出現「未知軟分叉」的警告訊息。詳情請看[Version Bits FAQ][2]。

如果你不依從以上指引，你可能會令所有支持BIP9的節點收到升級警報，將會造成很大混亂。

至於讓 bitcoind 自行設定區塊版本號的礦工則無需採取任何行動。請留意，你的節點在419328號區塊之前，仍然會產出0x20000001版本號的區塊，之後0號位元便會自動被去除。

## 使用或手動設定 Coinbase 交易內 nSequence 參數的礦工請注意

有些礦工可能會用 Coinbase 交易內的 nSequence 參數作為挖礦亂數。在 BIP68 生效之後，這些礦工需要加倍留意。

礦工如以任何形式改動 Coinbase 交易的 nSequence 參數，他們必須肯定 Coinbase 交易的 nVersion 參數 (*不是*區塊版本號) 必須為1或以下，例如把 nVersion 參數硬性地手動設定為1。

如果你不使用 Coinbase 交易內的 nSequence 參數，但必須手動設定，請設定為 0xffffffff。

不依從以上指引者可能會產出無效區塊，造成區塊鏈分叉，令有關礦工和廣大比特幣使用者蒙受經濟損失。

使用 bitcoind 提供的默認 Coinbase 交易 nSequence 和 nVersion 值的礦工無需採取任何行動。

## 使用或手動設定 Coinbase 交易內 nLockTime 參數的礦工請注意

這是比較罕見的情況，因為 Stratum 協議並不支持使用 nLockTime 參數作為挖礦亂數。然而任何這樣做的礦工必須留意 BIP113 的新規則。

礦工如以任何形式改動 Coinbase 交易的 nLockTime 參數，他們必須肯定參數值如被視作 UNIX 時間戳(即大或等於500000000)，必須小於過去11個區塊時間戳的中位數，除非 Coinbase 交易的 nSequence 參數同時為 0xffffffff。

如果你不使用 Coinbase 交易內的 nLockTime 參數，但必須手動設定，請設定為 0。

不依從以上指引者可能會產出無效區塊，造成區塊鏈分叉，令有關礦工和廣大比特幣使用者蒙受經濟損失。

使用 bitcoind 提供的默認 Coinbase 交易 nLockTime 值的礦工無需採取任何行動。

[1]: /en/contact/
[2]: /en/2016/06/08/version-bits-miners-faq/#when-should-miners-set-bits

{% include _references.md %}