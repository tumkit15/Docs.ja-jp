---
title: ASP.NET Core の分散キャッシュ タグ ヘルパー
author: pkellner
description: キャッシュ タグ ヘルパーを使用する方法を示します
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342173"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>ASP.NET Core の分散キャッシュ タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net) 

分散キャッシュ タグ ヘルパーは、ASP.NET Core アプリの内容を分散キャッシュ ソースにキャッシュすることによって、アプリのパフォーマンスを大幅に改善する機能を提供します。

分散キャッシュ タグ ヘルパーは、キャッシュ タグ ヘルパーと同じ基本クラスから継承されます。 キャッシュ タグ ヘルパーに関連付けられているすべての属性は、分散タグ ヘルパーでも機能します。

分散キャッシュ タグ ヘルパーは、**コンストラクター インジェクション**と呼ばれる**明示的な依存関係の原則**に従います。 具体的には、`IDistributedCache` インターフェイス コンテナーは、分散キャッシュ タグ ヘルパーのコンストラクターに渡されます。 `ConfigureServices` に `IDistributedCache` の具体的な実装が作成されていない場合 (通常は startup.cs にあります)、分散キャッシュ タグ ヘルパーはキャッシュされたデータの格納に基本キャッシュ タグ ヘルパーと同じメモリ内プロバイダーを使用します。

## <a name="distributed-cache-tag-helper-attributes"></a>分散キャッシュ タグ ヘルパーの属性

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority が有効

定義については、キャッシュ タグ ヘルパーを参照してください。 分散キャッシュ タグ ヘルパーはキャッシュ タグ ヘルパーと同じクラスから継承されるため、これらすべての属性がキャッシュ タグ ヘルパーと共通しています。

- - -

### <a name="name-required"></a>名前 (必須)

| 属性の型    | 値の例     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

必須の `name` 属性は、分散キャッシュ タグ ヘルパーのインスタンスごとに保存されているキャッシュのキーとして使用されます。 基本キャッシュ タグ ヘルパーは Razor ページ名と、Razor ページのタグ ヘルパーの場所に基づいて各キャッシュ タグ ヘルパー インスタンスにキーを割り当てますが、分散キャッシュ タグ ヘルパーのキーが基準とするのは属性 `name` のみです

使用例:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分散キャッシュ タグ ヘルパー IDistributedCache の実装

ASP.NET Core には `IDistributedCache` の 2 つの実装が組み込まれています。 1 つは SQL Server に基づき、もう 1 つは Redis に基づきます。 これらの実装の詳細については、<xref:performance/caching/distributed> を参照してください。 どちらの実装も、ASP.NET Core の *Startup.cs* で `IDistributedCache` のインスタンスの設定を伴います。

`IDistributedCache` のいずれかの具体的な実装の使用に、明確に関連付けられている属性はありません。

## <a name="additional-resources"></a>その他の技術情報

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
