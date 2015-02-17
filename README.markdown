#実験手順
1. prepreprocess.pyにより実験対象とする期限の限定と各tweetのフィルタリング（8 tweet以上しているユーザのtweetでなおかつ条件に合った単語が3つ以上含まれているtweet）を行う
```
$ python3.3 prepreprocess.py
```
1. preprocess.pyにより10 tweet以上に現れない単語，あるいは70%以上のtweetに現れる単語を除去する
```
$ python3.3 preprocess.py
```
1. 以下の作業をトピック数30，100，200，500のそれぞれについて行う
 - Twitter-LDAによりトピックを抽出
```
# EclipseからTwitterLDAmainを実行
```
 - 抽出結果を別テーブルに保存
 - キーワードを抽出して画像へアノテーション(各トピックごとに計算）
```
$ python3.3 extract_exp_lda_images.py
```
 - 抽出結果を別テーブルに保存
1. 各トピックごとにアノテーションした画像集合の重なり部分を抽出する
```
$ python3.3 select_tweet_for_exp.py
```
1. 抽出した重なり部分から全名詞数が一定数以下のtweetをM個取り出す
```
$ python3.3 limit_tweet_for_exp.py
```
1. 抽出した名詞に問題のある画像がないか検閲する
```
# Djangoサーバを起動して「/evaluate/censorship/」へアクセスする
```

##評価手順
1. evaluate_resultsテーブルから多数決をとることで正解データを作成してテーブルに保存
```
$ python3.3 make_answer.py
```
1. 提案手法がアノテーションした各単語の◯，×を確かめる
```
$ python3.3 calc_final_result.py
```
 - ◯だけの画像の割合
 - ×だけの画像の割合
 - ◯の含有率の平均
 - 全画像中の◯と×それぞれの割合
 - 各画像についてどれだけの×が残っているかの割合の平均
1. 元データの各画像がもつノイズ数の分布を算出
```
$ python3.3 make_histgram.py
```
1. 正解データについて，全ての名詞をそのままアノテーションしたときの以下のデータを計算する(calc_naive_result.pyを利用)
 - アノテーション精度
 - ノイズ除去精度
 - 正解データを含む画像の割合
```
$ python3.3 calc_naive_result.py
```
1. 名詞をそのままアノテーションしたときのカバー率を計算する
 - 100%（名詞がついてるものしかデータセットに用いていないため）

