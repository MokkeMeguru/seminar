

# 導入

> 対話システムなんもわからん  

対話システムとはなんぞやという疑問を持ち始めてはや数年、未だによくわからず、最近は画像やらに TF2.0betaの勉強やらに逃げている今日この頃でございます。  

しかしながらいずれかはやらなければならないわけで、よし、やってみるかとまず手を付けてみたのがまず **Knowledge Base** というものでした。こいつはどこで役に立つのかというと、[以前書いたAmazon Alexa の内部システムについての記事](https://qiita.com/MeguruMokke/items/83b3d921729b62ae8ee2) や [以前書いた対話システム今についての記事](https://qiita.com/MeguruMokke/items/6e5d7997f4df7f08030d) の話で登場する、 **対話システムにおける回答の核を作る** 部分となります。  

具体的に例を挙げるために X さんが対話システム Alice に 「令和0年時の日本の首相はだれ？」というような質問をしたとします。すると Alice は Language Understanding を経由することで、「<request\_who, who=首相, when=日本, when=令和0年>」というようなクエリに変換します(この辺りはTOEICの長文問題を想起しますね)。次に Alice は Dialog Management はこのクエリを受け取り、 **Knowledge Base** に回答を問い合わせます。すると Knowledge Base はそこから 「<answer=安倍>」みたいなものを返します。この結果を元に Natural Language Generation は自然な文 「その首相の名は、安倍さんだと思いますにゃん」を生成します。これを X さんは音声ないしテキストで Alice から受け取る、という仕組みになっています。(実際はそれぞれもっと色々やっています)  

そんなわけでパット見一番重要そうな **Knowledge Base** をやってみよう、ということになったのですが、どうやらこいつ、 **かなりふわっとしている** っぽい。言い換えると **定義がよくわかんない** 。特に **Knowledge Base** と **Knowledge Graph** の違いがわからん。  

そんなわけで良い記事を探して三千里していたところ見つけたのが、この記事、[WTF is a knowledge graph?](https://hackernoon.com/wtf-is-a-knowledge-graph-a16603a1a25f) とこの記事で引用されている論文 [Towards a Definition of Knowledge Graphs ](http://ceur-ws.org/Vol-1695/paper4.pdf) となります。  


# Google Knowledge Graph

Knowledge Graph の研究に最も影響を与えたそれは、おそらく Google Knowledge Graph でしょう。実際 Knowledge Graph をぱっと調べるとこの製品についてレコメンドされると思います。  

お使いの (特にPC/Macの) Google Chrome / Chromium / Canary で著名人の名前を検索すると左側に出てくるあれです。2012年に [Google が公表した Knowledge Graph に関する記事](https://googleblog.blogspot.co.uk/2012/05/introducing-knowledge-graph-things-not.html) は以下のようにして Knowledge Graph を定義しています。  

> &#x2026; we've been working on an intelligent model - in geek-speak, a 'graph' -  that understands real-workd entities and their relationships to one another: things, not strings.  
> 私達は現実の実体とそれらの相互関係、つまり文字列としてのそれではなく事物としてのそれを理解する知能モデルについて取り組んできました。そのモデルは、オタクっぽく表現するならばグラフです。  

この声明は全くと言って良いほどに Google Knowledge Graph の動作やその詳細については触れられていません。しかしこれは Wikipedia などの大規模な公開されている資源や、(おそらく)我々の検索履歴などを利用していると想定されており、それは700億のエッジを持った複雑なグラフ構造(2016年度)であるらしいと [言われています](http://www.theverge.com/2016/10/4/13122406/google-phone-event-stats)。  


# Knowledge Graph の定義

Google Knowledge Graph という製品から、一般的な Knowledge Graph に話題を戻しましょう。Wikipedia 自体にそのリンクは(この記事が書かれた当時)なく、代わりに以下の2つのページがヒットします。  

-   [ontology(オントロジー)](https://en.wikipedia.org/wiki/Ontology_(information_science)) に関するページ。狂気的なほどに複雑です。
-   [semantic network(意味ネットワーク)](https://en.wikipedia.org/wiki/Semantic_network) に関するページ。触れたくもありません。

まあ、これらの記事から推察するに、どうやら Knowledge Graph というものは意味論的なコンテクストで行われるデータの結合についての技術的産物、つまりグラフ構造をベースとした知識の表現手法の一種であるらしいです。 **しかしこれでは明瞭な解釈とは言えないでしょう。**  

ラッキーなことに、この意識は他の研究者達も持っているようで、Knowledge Graph の定義に関する [論文](http://ceur-ws.org/Vol-1695/paper4.pdf) Towards a Definition of Knowledge Graph が発表されていました。この論文では "勿論他の解釈もあるかもしれないが" と様々な意見に留意した上で、それらの意見を集計した上で発表しています。  


## Knowledge Graph Refinement: A Survey of Approaches and Evaluation Methods の場合

> A knowledge Graph mainly describes real world entities and their interrelations, organized in a graph, defines possible classes and relations of entities in a schema,  allows for potentially interrelation arbitrary entities with each other and covers various topical domains.  

Knowledge Graph は、主に現実の実体らの相互関係をグラフで整理し、実体に関する関係とクラス関係(親/子のようなもの？)をスキーマを用いて定義し、相互に関係のある任意のエンティティを互いに関連付けることを可能にし、様々な話題のドメインをカバーすることができるものである。  


## Journal of Web Semantics: Special Issue on Knowledge Graphs の場合

> Knowledge graphs are large network of entities, their semantic types, properties,  and relationships between entities.  

Knowledge Graph は実体自身について、あるいは実体に関する意味論的な型、実体の性質、実体同士の関係についてを表現した大規模ネットワークである。  


## From Taxonomies over Ontologies to Knowledge Graphs の場合

> Knowledge graph could be envisaged as a network of all kind things which are relevant to a specific domain  or to an organization. They are not limited to abstract concepts and relations but can also contain instances of things like documents and datasets.  

Knowledge Graph は特定のドメインや組織に関するされたすべての事物に対するネットワークとして構想される。それらは抽象的な概念や関係に限定されず、文書やデータセットについてもインスタントしてみなすことができる。  


## Linked Data Quality of DBpedia, Freebase, OpenCyc, Wikipedia, and YAGO, Semantic Web  Journal の場合

> We define a Knowledge Graph as an RDF graph. An RDF graph consists of a set of RDF triples where each RDF triple (s, p, o) is an ordered set of the following RDF terms: a subjects \(\in\)  U \(\cup\) B , a predicate p \(in\)  U, and and object U \(\cup\) B \(\cup\) L. An RDF term is either a URI u \(\in\) U, a blank node b \(\in\) B or a literal l \(\in\) L.  

Knowledge Graph とは RDF のグラフである。RDF Graph とは RDF triple の集合によって構成され、RDF triple(s, p, o) は次の RDF 項によって表される順序付き集合である。  
s とは subject (主題) であり、 U \(\cup\) B の要素である。  
p とは predicate (述語) であり、U の要素である。  
o は object (対象) であり、 \(U\cup B \cup L\) (の要素) である。  
つまり RDF 項は URI u \(\in\) U または空ノード b \(\in\) B または逐語 l \(\in\) L のいずれかの要素である。  

RDF については [こちらのスライド](https://www.slideshare.net/oracle4engineer/rdf-semantic-graph-intro) を参考にすると良いでしょう。(正しい理解であるかは不明ですが、理解の助けにはなると思います)  


## Knowledge Graph Identification の場合

> [&#x2026;] systems exist, [&#x2026;], which use a variety of techniques to extract new knowledge, in the form of facts, from the web. These facts are interrelated, and hence, recently this extracted knowledge has been referred to as a knowledge graph.  

Knowledge Graph とは新たな知識を抽出するための様々な技術を指しており、これはWeb からの事実を元にして構築される。これらの事実は相互に関連性があるため、最近よりこの抽出された知識を Knowledge Graph とみなすことになっている。  


# Re: Knowledge Graph の定義

> A knowledge graph acquires and integrates information into an ontology and applies a reasoner to derive new knowledge  

**知識グラフは情報を獲得してオントロジーへ統合し、新たな知識を引き出すために推論を行うものである**  

上述のこれが Towards a Definition of Knowledge Graph で新たに提案された Knowledge Graph の定義です。  


# Knowledge Graph の概要

この章では記事で述べられていた、Knowledge Graph とはどんな感じでどう嬉しいのか、みたいなことを紹介します。  


## Knowledge Graph とは グラフ である

つまり Knowledge Graph と Knowledge Base とはこのグラフであるかどうかという一点が重要な違いを示すファクターになっています。  

尚注意しなければならないこととして、 Knowledge Graph は Knowledge Base を示すこともあれば Knowledge Base を利用する可能性があるということです。  

つまり両者は必ずしも全くの別物として捉えることはできないということです。  

ただし繰り返しますが、Knowledge Graph はデータ内の接続(関係)が必ず第一級オブジェクトです。  

グラフを用いる利点は、新たなデータ項目がデータプールに追加された際には簡単にそれらを追加することができ、またリンクをたどることでそれぞれのドメインの関係を調べることができるという点です(このリンクに含まれる情報には非常に価値があると考えられています)。またグラフとは最も柔軟な形式データ構造の一種であるので、一般的なツールやパイプラインを用いることで、外部データを比較的簡単に統合することができる点も利点でしょう。  


## Knowledge Graph とは 意味論的なコンテクスト で用いられる技術である

semantic とは極めて専門性の高い用語です。あんまり適用なことを言うと別の研究分野どころか別の学問分野にまで飛び火する危険用語です。semantic meaning なんて出てきた暁には手足を縛られて死海に沈められる[覚悟の準備をしなければなりません](https://dic.nicovideo.jp/a/ワザップジョルノ) 。  

おおよそに説明すると、あるデータに関する意味とは、オントロジーの形で表され、でグラフ内のデータと一緒に埋め込まれているものです。Knowledge Graph とは自己記述(self-descriptive)的な側面があり、噛み砕いて言うならば、データを見つけてそのデータの意味を理解するための一つの空間という風に表現できます。  


## Knowledge Graph とは 推論を行うことができる賢いものである

Knowledge Graph の基礎としてみなされるものは、ontology です。ontology とはデータの意味を示しており、これは通常、何らかの形の推論を補助する論理形式に基づいています。つまり暗黙の情報を明示的に表されているデータから導き出すことを可能にしているということです。  

また推論された情報中には、このような手法を用いなければ見つけることが不可能なような関係性が含まれる可能性があります。(これがKnowledge Graphに期待される特性の一つとして挙げることができるでしょう)  

Knowledge Graph は数学的に適切なグラフ構造を取ることが一般的であり、このために最短経路問題やネットワーク解析などの様々なグラフ構造のための問題に落ち着かせることができます。  

またSQLのような厳密すぎず無茶苦茶なものでもない適当なスキーマを用いることで、逐次的にデータのやり取りを行うことができます。これによって Knowledge Graph は時間をかけて拡張することが可能になっています。  


## Knowledge Grpah  とは 生きているものである

Knowledge Graph は柔軟な構造をしているため、新たなデータに対してオントロジーを拡張し、そのグラフを修正することができます。 **定期的な更新** と **定期的なデータの増加** が重要な場合において、特にデータが様々なデータソースから引っ張ってこられている場合には、データを Knowledge Graph の形に落とし込むことは管理を簡易化することができると考えられます。Knowledge Graph はグラフに新たな知識を追加し続けることができ、自身を改良していくことができます。またそれを継続的に行うデータパイプラインをサポートすることができます。  

また Knowledge Graph は情報の出処やバージョン情報などの多様なメタデータを取り込むことが可能なので、動的なデータセットを扱うことに理想であると言えます。信頼性のある情報であることを示すために、データの出所を説明できる必要性が高まっている現在、Knowledge Graphのこの利点は非常に価値のあるものであると言えます。  


# 感想とか

Knowledge Graph ってすげー(小学生並みの感想)  

データ表現の一種としてこれをみなしていた節があるので、Knowledge Graph の要件として推論を強調する、というのは先を見ている感じがして面白いなと思いました。  

ただデータで表現できないような未知のデータの関係を導き出す、というよりはデータに欠落している情報を補完する、という機能のほうが例えば Twitter などのデータソースの利用を考えた際には必要なのかな、と思いました。勿論 Wikipedia なんかも良いデータソースだとは思いますが、例えば対話システムなんかでの利用を考えた際には、流行などの言葉にはできるかもしれないけど雰囲気で察して欲しい内容について強くしたいです。  

次からこの方面では手法とかアルゴリズム、実アプリケーションの例について触れていきたいと思います。  


# 追記

他方面の記事が遅れていますが、単純に忙しいせいです。興味がなくなったわけではないです。  

