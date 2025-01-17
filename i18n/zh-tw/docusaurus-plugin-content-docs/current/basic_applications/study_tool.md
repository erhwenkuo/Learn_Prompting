---
sidebar_position: 30
---

# 🟢 學習小幫手

我們最常見的對 LLMs 的使用方式之一就是將其作為學習工具，LLMs 在解釋令人困惑的術語時是非常有幫助的。

## 解釋術語

最近我在閱讀一本關於仙人掌（Cacti）的教材(@nobel2002cacti)，看到了這段文字。:

```text
Because of adaptations to xeric conditions and the presence of **less derived woods** in Pereskioideae, the Cactaceae is one of the most interesting families both anatomically and morphologically. 
```

我不知道 **less derived woods** 是什麼意思，Ｇoogle 上只有研究文章。我決定使用 ChatGPT 找出它的含義。我給了它下面這個提示，也就是書中的一段話和我的問題。

```text
Because of adaptations to xeric conditions and the presence of **less derived woods** in Pereskioideae, the Cactaceae is one of the most interesting families both anatomically and morphologically. 

在上述段落中，less derived woods 是什麼意思?

// highlight-start
在上述段落中，"less derived woods" 的意思是指「不那麼演化的木質部分」或「較為原始的木質結構」。在這個脈絡中，它指的是仙人掌科（Cactaceae）植物的木質部分相對較為原始或未經過高度演化的特徵。這可能與仙人掌科植物適應乾旱條件的特殊演化有關，使得它們在解剖結構和形態學上變得非常有趣。
//highlight-end
```

可見模型可以快速解答我的疑問從而使我可以繼續後續的閱讀。

## 自問自答

另一種將 LLM 作為學習工具的方式是進行自測。如果你即將參加微積分考試，你可以簡單地提問:

```text
生成 5 道微積分測驗題目：

// highlight-start
1. 求 y = x3 的導數是什麼？
2. 求不定積分 x3 + 5x2 + 7x + 4 的值。
3. 求導數的乘法法則是什麼？
4. 求函式 y = x2 在點 (4, 16) 處的切線方程。
5. 求 y = x4 的二階導數是什麼？
//highlight-end
```

如果你正在學習更具體的課程，那麼你可能需要給模型提供額外的資訊，例如你的課堂筆記！

```text
3/20/2020 2:30 下午

市場營銷課堂筆記

- 討論了市場營銷的4P理論：產品、價格、渠道、推廣
- B2C和B2B的區別：B2C是直接面向消費者市場，B2B則是企業間的市場
- 不同型別的市場研究：定量和定性研究，如調查和焦點小組討論
- 定價策略：撇除、滲透、競爭、價值等
- 分割市場：按照年齡、收入等因素對客戶進行細分
- 4種市場細分：人口統計、地理、心理學、行為學
- 討論了不同的促銷工具：廣告、個人銷售、公共關係、直銷和銷售促銷

基於我的課堂筆記請生成 5 道市場營銷的測驗題目:

// highlight-start
1. 市場營銷的四個 P 是什麼?
2. B2C 和 B2B 的區別是什麼?
3. 市場研究有哪兩種型別?
4. 列舉四種細分市場的型別?
5. 列舉五種促銷工具的名稱.
//highlight-end
```

## 總結

除了以上方法外，當然還有其他的方式利用 LLMs 來學習。像 Duolingo 和 Quizlet 這樣的學習公司就正在將 LLMs 引入他們的產品中。

By [gezilinll](https://github.com/gezilinll).