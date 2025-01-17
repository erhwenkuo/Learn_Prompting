---
sidebar_position: 5
---

# 🟡 提示多樣性

DiVeRSe(@li2022advance) ("**Di**verse **Ve**rifier on **R**easoning **S**t**e**ps") 是一張透過三重方式提高答案可靠性的方法。它透過使用多個提示生成多樣化的補全結果（completions），使用驗證器區分好的答案和壞的答案，並使用驗證器檢查推理步驟的正確性。

import diverse from '@site/docs/assets/reliability/diverse.webp';

<div style={{textAlign: 'center'}}>
  <img src={diverse} style={{width: "750px"}}/>
</div>

<div style={{textAlign: 'center'}}>
DiVeRSe (Li et al.)
</div>


## 提示多樣性

DiVeRSe使用五個不同的提示來對給定的輸入進行編碼。為了構造每個提示，他們隨機從訓練集中抽取幾個樣例。以下是一個樣本（k = 2）的少量資料提示，其中樣例取自[GSM8K 基準測試](https://raw.githubusercontent.com/openai/grade-school-math/master/grade_school_math/data/train.jsonl)(@cobbe2021training)。在此基準測試中，DiVeRSe使用5個樣例來構建提示。

```
Q：Natalia在4月份向她的48個朋友出售了夾子，然後在5月份賣出了一半。 Natalia在四月和五月共賣了多少個夾子？
A：Natalia在五月份賣出了48/2 = 24個夾子。
Natalia在四月和五月共賣出了48 + 24 = 72個夾子。
#### 72
Q：Weng為照看孩子每小時賺取12美元。昨天，她只照看了50分鐘。她賺了多少錢？
A：翁每分鐘賺12/60 = 0.2美元。
工作50分鐘，她賺了0.2 x 50 = 10美元。
#### 10
Q：貝蒂正在為一隻價值100美元的新錢包存錢。貝蒂只有一半的錢。她的父母決定給她15美元，並且她的祖父母比她的父母多兩倍。貝蒂還需要多少錢才能買錢包？
A：
```

在生成類似上面的5個不同提示之後，DiVeRSe為每個提示（temperature = 0.5）取樣20條推理路徑。以下是上述提示的幾個樣本的補全結果。請注意，答案並不總是正確的。

<pre>

<div style={{backgroundColor: '#d2f4d3'}}>
<div>Betty有0.5*100 = $50。</div>

<div>然後她又得到了15美元 = $65。</div>

<div>然後她又得到了2*15美元 = $90。</div>

<div>她還需要100-90 = $10。</div>

#### 10
</div>
</pre>
<pre>

<div style={{backgroundColor: '#d2f4d3'}}>
<div>A: Betty有0.5*100 = $500。</div>

<div>然後她又得到了15美元 = $650。</div>

<div>然後她又得到了2*15美元 = $900。</div>

<div>她還需要100-90 = $1000。</div>

#### 1000
</div>
</pre>

此時，DiVeRSe已經生成了100個不同的完成。
## 投票驗證器

現在，我們可以像自洽性(@mitchell2022enhancing)一樣，直接採用多數答案。

但是，DiVeRSe提出了一種更復雜的方法，稱為_投票驗證器_。

在測試時，使用投票驗證器是一個兩步過程。首先，驗證器（一個神經網路）根據其可能正確的機率為每個補全結果分配0-1分數。然後，“投票”元件對不同答案的所有分數進行求和，併產生最終答案。

### 樣本

這裡是一個小例子。假設對於`二加二等於幾？`這個提示的補全結果是這樣的：
<pre>
<div style={{backgroundColor: '#d2f4d3'}}>
<div>4</div>
</div>
</pre>

<pre>
<div style={{backgroundColor: '#d2f4d3'}}>
<div>二 + 2 = 5</div>
</div>
</pre>

<pre>
<div style={{backgroundColor: '#d2f4d3'}}>
<div>我想 2+2 = 6</div>
</div>
</pre>

<pre>
<div style={{backgroundColor: '#d2f4d3'}}>
<div>二加二 = 4</div>
</div>
</pre>

<pre>
<div style={{backgroundColor: '#d2f4d3'}}>
<div>答案是5</div>
</div>
</pre>

驗證器將讀取每個補全結果併為其分配分數。例如，它可能分配以下分數：0.9，0.1，0.2，0.8，0.3。然後，“投票”元件將對每個答案的分數進行求和。

```
score(4) = 0.9 + 0.8 = 1.7
score(5) = 0.1 + 0.3 = 0.4
score(6) = 0.2
```

最終答案是4，因為它具有最高的分數。

**但驗證器是如何訓練的？**

驗證器使用稍微複雜的損失函式進行訓練，在這裡不進行詳細介紹。請閱讀論文3.3節以獲取更多細節(@li2022advance)。

## 總結

這裡主要是使用多個提示來生成多樣化的補全結果。在實踐中，與投票驗證相比，多數投票可能效果更好。