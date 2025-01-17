---
sidebar_position: 2
---

# 🟢 提示洩漏


提示洩漏(Prompt leaking)是一種提示注入的形式，其中模型被要求輸出自己的提示。

如下面的範例圖片(@ignore_previous_prompt) 所示，攻擊者更改 `user_input` 以嘗試返回提示。提示洩漏的意圖和目標劫持（常規 prompt injection）不同，提示洩漏透過更改 `user_input` 以列印惡意指令(@ignore_previous_prompt)。

import research from '@site/docs/assets/jailbreak/jailbreak_research.webp';

<div style={{textAlign: 'center'}}>
  <img src={research} style={{width: "500px"}}/>
</div>

以下圖片(@simon2022inject)再次來自 `remoteli.io` 的範例，展示了 Twitter 使用者如何讓模型洩漏其提示。

import Image from '@site/docs/assets/jailbreak/injection_leak.webp';

<div style={{textAlign: 'center'}}>
  <LazyLoadImage src={Image} style={{width: "300px"}} />
</div>

那又怎麼樣？為什麼有人要關心提示洩漏呢？

有時人們想保守他們的提示秘密。例如，一家教育公司可能正在使用提示`用 5 歲小孩能聽懂的方式解釋這個`，來解釋複雜的主題。如果提示洩漏了，那麼任何人都可以使用它，而不必透過該公司。

隨著基於 GPT-3 的新創公司的不斷湧現，他們的提示更加複雜，需要耗費數小時的開發時間，提示洩漏成為了一個真正的問題。

## 練習

嘗試透過向提示新增文字來洩漏以下提示(@chase2021adversarial)：

<iframe
    src="https://embed.learnprompting.org/embed?config=eyJ0b3BQIjowLCJ0ZW1wZXJhdHVyZSI6MCwibWF4VG9rZW5zIjoyNTYsIm91dHB1dCI6IiIsInByb21wdCI6IkVuZ2xpc2g6IEkgd2FudCB0byBnbyB0byB0aGUgcGFyayB0b2RheS5cbkZyZW5jaDogSmUgdmV1eCBhbGxlciBhdSBwYXJjIGF1am91cmQnaHVpLlxuRW5nbGlzaDogSSBsaWtlIHRvIHdlYXIgYSBoYXQgd2hlbiBpdCByYWlucy5cbkZyZW5jaDogSidhaW1lIHBvcnRlciB1biBjaGFwZWF1IHF1YW5kIGl0IHBsZXV0LlxuRW5nbGlzaDogV2hhdCBhcmUgeW91IGRvaW5nIGF0IHNjaG9vbD9cbkZyZW5jaDogUXUnZXN0LWNlIHF1ZSB0byBmYWlzIGEgbCdlY29sZT9cbkVuZ2xpc2g6IiwibW9kZWwiOiJ0ZXh0LWRhdmluY2ktMDAzIn0%3D"
    style={{width:"100%", height:"500px", border:"0", borderRadius:"4px", overflow:"hidden"}}
    sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>
