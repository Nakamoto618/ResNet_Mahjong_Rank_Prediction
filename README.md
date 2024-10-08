# ResNet_Mahjong_Rank_Prediction

# ResNetを用いた麻雀の着順分類モデル

このリポジトリは、麻雀の対局データを画像データとして変換し、ResNetを用いてプレイヤーの着順を予測するプロジェクトです。ディープラーニングを用いて麻雀の対局情報を視覚的に捉え、ゲーム状態のパターンを学習することを目指しています。

## 目次

- [概要](#概要)
- [データについて](#データについて)
- [データ準備](#データ準備)
- [モデルアーキテクチャ](#モデルアーキテクチャ)
- [学習と評価](#学習と評価)
- [結果](#結果)
- [今後の展望](#今後の展望)
- [参考文献](#参考文献)

## 概要

麻雀はその複雑なルールと確率的な要素により、対局結果の予測が難しいゲームです。本プロジェクトでは、対局データを画像データに変換し、CNN（畳み込みニューラルネットワーク）の一種であるResNetを用いてプレイヤーの着順予測を行います。

このモデルでは、各局面を画像形式で表現し、画像処理技術を応用することで、対局情報を効果的に学習させることを目的としています。

## データについて

使用したデータはオンライン麻雀サービス「天鳳」よりお借りしました。本プロジェクトは研究目的でデータを利用していますが、天鳳のライセンス規約に抵触する可能性があるため、公開は控えさせていただきます。

## データ構造

麻雀の対局データは、各種状態やプレイヤーの行動を多チャンネルの画像形式に変換し、以下のような形で表現されています：

- **データ形状**： `(93678, 4, 9, 392)`
  - **チャンネル数 (392)**：牌情報、プレイヤーの行動、局面の状態などの各種情報
  - **高さ(4)と幅(9)**：牌種をチャンネルとして、牌を9(数値または字牌の種類)×4(牌数)の2次元の面で表現

これらの画像データを使用し、ResNetモデルに入力することで、局面ごとのパターンを学習し、プレイヤーの最終的な着順を予測します。

## モデルアーキテクチャ

本プロジェクトでは、麻雀の局面の空間的な配置を保つため、プーリング層を使用せず、位置情報を重視したResNetを使用しています。主なモデル構成は以下の通りです：

- **Residual Block（残差ブロック）**：複雑なパターンや相関関係を捉えるために使用
- **全結合層（Fully Connected Layer）**：残差ブロックで抽出された特徴量を結合し、分類を行う
- **Softmax 出力層**：4プレイヤーの着順分布を予測する確率分布を出力
- **ResNet50**を使用

## 学習と評価

本モデルは、以下の方法で学習および評価を行います。

- **損失関数**：クロスエントロピー損失関数（Categorical Cross-Entropy）を使用し、着順を4クラスの分類問題として扱う
- **最適化アルゴリズム**：Adam Optimizerを採用し、学習率調整などを行う
- **評価指標**：各着順に対するROC AUCスコアを算出し、モデルの性能を評価している

## 結果

本モデルの評価結果は以下の通りです：

- **マクロ平均 ROC AUC スコア**：0.83
- **マイクロ平均 ROC AUC スコア**：0.83
- **重み付き ROC AUC スコア**：0.83
- **各クラスごとのスコア**：
  - 1位：0.87
  - 2位：0.77
  - 3位：0.79
  - 4位：0.88
- **各クラスの平均スコア**：0.83

これらの結果は、麻雀の着順予測タスクにおいて、一定のパフォーマンスを示しています。特に1位と4位の予測精度が高く、他のクラスに対しても安定した予測を行えていることがわかります。


## 今後の展望

- **モデルの課題**：
  - ResNetは局所的なパターンを抽出することに長けているため、ある程度の着順予測は行うことができました。しかし、以下の課題が見られました：
    - **着順が確定している局面でも判断できない**：
      - ゲームの進行中、着順がほぼ確定した局面においても、モデルがそれを正確に判断できず、安定した予測結果を出力し続けてしまいます。
    - **局面の細かな変化に反応できない**：
      - 対局の進行中に、局面が変化しても、モデルはおおよその形成（例えば「中間の順位」や「僅差の順位」を示す傾向）を出力し続けることがあり、着順変動の予測が不十分です。
  
- **解決策：Transformerモデルの導入**：
  - これらの課題を解決するために、Transformerの導入を検討しています。Transformerは、自己注意機構（Self-Attention Mechanism）を用いることで、長期的な依存関係や文脈の理解に優れており、局面の全体的な情報を考慮しながら学習することが可能です。

- **実用化に向けた取り組み**：
  - リアルタイム予測システムの開発と最適化を進め、プロの放送対局でのリアルタイム着順予測を目指します。
  - GUI を通じて、対局の進行を可視化し、ユーザーが予測結果を視覚的に理解できるインターフェースを構築します。

## 参考文献

- 天鳳公式サイト: [https://tenhou.net/](https://tenhou.net/)
- データ構造を決める際に参考にしたブログ: [https://tadaoyamaoka.hatenablog.com/entry/2023/10/09/233816](https://tadaoyamaoka.hatenablog.com/entry/2023/10/09/233816)
















