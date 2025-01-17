---
sidebar_position: 4
---

# 🟢 零樣本思維鏈

零樣本思維鏈（Zero Shot Chain of Thought，Zero-shot-CoT）提示過程(@kojima2022large)是對 %%CoT prompting|CoT prompting%% (@wei2022chain) 的後續研究，引入了一種非常簡單的零樣本提示。他們發現，透過在問題的結尾附加 “**讓我們一步步思考 (Let's think step by step)**” 這一句話，大語言模型能夠生成一個回答問題的思維鏈。從這個思維鏈中，他們能夠提取更準確的答案。

import ZSImage from '@site/docs/assets/intermediate/zero_shot.webp';

<div style={{textAlign: 'center'}}>
  <img src={ZSImage} style={{width: "500px"}}/>
</div>
<div style={{textAlign: 'center'}}>
零樣本思維鏈(Kojima et al.)
</div>

從技術上講，完整的零樣本思維鏈過程涉及兩個單獨的提示/補全結果。在下面的圖像中，左側的提示生成一個思維鏈，而右側的頂部氣泡接收來自第一個提示（包括第一個提示本身）的輸出，並從思維鏈中提取答案。這個第二個提示是一個**自我增強**的提示。

import ZSProcessImage from '@site/docs/assets/intermediate/zero_shot_example.webp';

<div style={{textAlign: 'center'}}>
  <LazyLoadImage src={ZSProcessImage} style={{width: "500px"}} />
</div>
<div style={{textAlign: 'center'}}>
完整的零樣本思維鏈過程(Kojima et al.)
</div>

## 範例

以下是一些演示（僅執行推理提取）。這個演示展示了GPT-3（davinci-003）無法解決一個簡單的數學問題，而第二個演示使用了零樣本思維鏈提示，併成功地解決了這個問題。隨意輸入您的OpenAI API金鑰（點選生成），並在範例中進行操作。請注意，與思維鏈提示相比，零樣本思維鏈提示要簡單得多。

#### 錯誤範例

<iframe
    src="https://embed.learnprompting.org/embed?config=eyJ0b3BQIjowLCJ0ZW1wZXJhdHVyZSI6MCwibWF4VG9rZW5zIjoyNTYsIm91dHB1dCI6IkpvaG4g5pyJIDgg5Liq5qKo5a2Q44CCIiwicHJvbXB0Ijoi5aaC5p6cIEpvaG4g5pyJIDUg5Liq5qKo5a2Q77yM5ZCD5LqGIDIg5Liq77yM5Y%2BI5Lmw5LqGIDUg5Liq77yM54S25ZCO5oqKIDMg5Liq57uZ5LqG5LuW55qE5pyL5Y%2BL77yM5LuW6L%2BY5Ymp5LiL5aSa5bCR5Liq5qKo5a2Q77yfIiwibW9kZWwiOiJ0ZXh0LWRhdmluY2ktMDAzIn0%3D"
    style={{width:"100%", height:"500px", border:"0", borderRadius:"4px", overflow:"hidden"}}
    sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

#### 正確範例

<iframe
    src="https://embed.learnprompting.org/embed?config=eyJtb2RlbCI6InRleHQtZGF2aW5jaS0wMDMiLCJwcm9tcHQiOiLlpoLmnpwgSm9obiDmnIkgNSDkuKrmoqjlrZDvvIzlkIPkuoYgMiDkuKrvvIzlj4jkubDkuoYgNSDkuKrvvIznhLblkI7mioogMyDkuKrnu5nkuobku5bnmoTmnIvlj4vvvIzku5bov5jliankuIvlpJrlsJHkuKrmoqjlrZDvvJ9cblxu6K6p5oiR5Lus5LiA5q2l5LiA5q2l5Zyw5oCd6ICD44CCIiwib3V0cHV0IjoiSm9obiDotbfliJ3mnIkgNSDkuKrmoqjlrZDjgILku5blkIPkuoYgMiDkuKrmoqjlrZDvvIzov5jliankuIsgMyDkuKrmoqjlrZDjgILku5blj4jkubDkuoYgNSDkuKrmoqjlrZDvvIzkuIDlhbHmnIkgOCDkuKrmoqjlrZDjgILku5bmioogMyDkuKrmoqjlrZDnu5nkuobku5bnmoTmnIvlj4vvvIzku5bnjrDlnKjlj6rliankuIsgNSDkuKrmoqjlrZDjgIIiLCJtYXhUb2tlbnMiOjI1NiwiYm94Um93cyI6NSwidGVtcGVyYXR1cmUiOjAuNywidG9wUCI6MX0%3D"
    style={{width:"100%", height:"250px", border:"0", borderRadius:"4px", overflow:"hidden"}}
    sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

## 結論
零樣本思維鏈也有效地改善了算術、常識和符號推理任務的結果。然而，毫不奇怪的是，它通常不如思維鏈提示過程有效。，在獲取思維鏈提示的少量範例有困難的時候，零樣本思維鏈可以派上用場。

## 有趣的試誤實驗

Kojima 等人嘗試了許多不同的零樣本思維鏈提示（例如 "Let’s solve this problem by splitting it into steps." 或 "Let’s think about this logically."），但他們發現 "Let's think step by step" 對於他們選擇的任務最有效。

## 備註

提取步驟通常必須針對特定任務，使得零樣本思維鏈的泛化能力不如它一開始看起來的那樣強。

從個人經驗來看，零樣本思維鏈型別的提示有時可以有效地提高生成任務完成的長度。例如，請考慮標準提示`寫一個關於青蛙和蘑菇成為朋友的故事`。在此提示的末尾附加`讓我們一步一步地思考`會導致更長的補全結果。