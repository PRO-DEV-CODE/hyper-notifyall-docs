# แจ้งเตือนและไอเทม

## sendNotify

การ์ดแจ้งเตือนทั่วไป เรียกใช้มากที่สุดในเซิร์ฟ — 79 resource

![การ์ดแจ้งเตือนพื้นฐาน](../.gitbook/assets/01_notify_types.png)

```lua
exports['Hyper_Notifyall']:sendNotify({
    text = {
        title = 'แจ้งเตือน',
        message = 'คุณอยู่ห่างจากรถ %s เกินไป',
        highlight = 'RRR 999',
    },
    type = 'error',
    time = 5 * 1000,
    layout = 'topRight',
})
```

| ค่า | ชนิด | ค่าเริ่มต้น | ความหมาย |
| --- | --- | --- | --- |
| `text.title` | string | `'แจ้งเตือน'` | หัวข้อการ์ด |
| `text.message` | string | `''` | เนื้อความ ใช้ `%s` เป็นจุดไฮไลต์ |
| `text.highlight` | string หรือ table | — | คำที่จะแทน `%s` ส่งเป็น table เพื่อแทนหลายจุดตามลำดับ |
| `type` | string | `'info'` | `info` `success` `error` `warning` `alert` `phone` `police` `medic` `council` `message` |
| `time` | number | `5000` | เวลาแสดง (มิลลิวินาที) |
| `layout` | string | `'centerRight'` | `topLeft` `topCenter` `topRight` `centerLeft` `centerRight` `bottomLeft` `bottomCenter` `bottomRight` |
| `sound` | table | จาก config | `{ enable = true, name = 'notify-normal.mp3', volume = 0.5 }` |
| `source` | number | — | ใส่เมื่อเรียกจากฝั่งเซิร์ฟเวอร์ |

ไฮไลต์หลายจุดพร้อมกัน

```lua
exports['Hyper_Notifyall']:sendNotify({
    text = {
        title = 'รายงาน',
        message = 'พบ %s ที่โซน %s',
        highlight = { 'ผู้ต้องสงสัย', 'A' },
    },
    type = 'police',
    time = 8 * 1000,
    layout = 'topRight',
})
```

เรียกจากฝั่งเซิร์ฟเวอร์

```lua
exports['Hyper_Notifyall']:sendNotify({
    text = { title = 'ธนาคาร', message = 'โอนเงินสำเร็จ' },
    type = 'success',
    time = 5000,
    layout = 'topRight',
    source = source,
})
```

{% hint style="info" %}
ส่งแบบสั้นก็ได้ — ถ้าไม่มี `text` ระบบจะอ่าน `title` `message` `highlight` จากชั้นบนสุดของตารางแทน
{% endhint %}

## sendMessageNotify

การ์ดข้อความเข้าในโทรศัพท์ ใช้ชนิด `message` เป็นค่าเริ่มต้น

![การ์ดข้อความ](../.gitbook/assets/02_notify_jobs.png)

```lua
exports['Hyper_Notifyall']:sendMessageNotify({
    text = {
        title = 'แจ้งเตือนข้อความ',
        message = 'จาก %s : เจอกันที่เดิมนะ',
        highlight = '081-234-5678',
    },
    type = 'message',
    time = 5 * 1000,
    layout = 'centerRight',
})
```

รับค่าชุดเดียวกับ `sendNotify` ต่างที่ค่าเริ่มต้นมาจาก `Config.Message_Notify`

## phoneNotify

การ์ดสายเรียกเข้า

![การ์ดสายเข้า](../.gitbook/assets/03_phone_call.png)

```lua
exports['Hyper_Notifyall']:phoneNotify({
    text = {
        message = 'กำลังมีสายเข้าจาก %s',
        highlight = '081-234-5678',
    },
    time = 5 * 1000,
    hold = true,
    layout = 'centerRight',
})
```

| ค่า | ชนิด | ความหมาย |
| --- | --- | --- |
| `text.message` | string | เนื้อความ |
| `text.highlight` | string | เบอร์ที่โทรเข้า |
| `hold` | boolean | `true` = การ์ดค้างจนกว่าจะสั่งปิด (`time` ไม่มีผล) |
| `time` | number | เวลาแสดง ใช้เมื่อ `hold` ไม่ได้เปิด |
| `layout` | string | มุมจอ |

## phoneNotifyClose

ปิดการ์ดสายเรียกเข้าที่ค้างอยู่

```lua
exports['Hyper_Notifyall']:phoneNotifyClose()
```

{% hint style="warning" %}
ถ้าเปิด `hold = true` ต้องเรียก `phoneNotifyClose()` ในทุกเส้นทางที่สายจบ — รับสาย ปฏิเสธ และเมินสาย ไม่งั้นการ์ดจะค้างบนจอ
{% endhint %}

## itemNotify

การ์ดไอเทมเข้า–ออก

![การ์ดไอเทมหลายชิ้น](../.gitbook/assets/05_item_batch.png)

```lua
exports['Hyper_Notifyall']:itemNotify({
    action = 'Item',
    label = 'ผ้าพันแผล',
    name = 'bandage',
    count = 6,
    type = 'info',
    time = 8 * 1000,
    layout = 'bottomRight',
})
```

| ค่า | ชนิด | ความหมาย |
| --- | --- | --- |
| `action` | string | `'Item'` `'Account'` หรือ `'Weapon'` — ชี้ว่าจะอ่านค่าเริ่มต้นจากบล็อกไหนใน config |
| `name` | string | ชื่อไอเทม ใช้หารูปและถามระดับความหายาก |
| `label` | string | ชื่อที่โชว์บนการ์ด |
| `count` | number | จำนวน |
| `type` | string | `'info'` = ได้รับ · ค่าอื่น (เช่น `'REMOVE'`) = เสียไป |
| `time` | number | เวลาแสดง |
| `layout` | string | มุมจอ |
| `rarityColor` | string | สีกรอบแบบกำหนดเอง ไม่ใส่ = ถามจากระบบกระเป๋า |
| `rarityTier` | string | ชื่อระดับความหายาก ไม่ใส่ = ถามจากระบบกระเป๋า |

ไอเทมออกจากตัว

![การ์ดไอเทมออก](../.gitbook/assets/06_item_removed.png)

```lua
exports['Hyper_Notifyall']:itemNotify({
    action = 'Item',
    label = 'โทรศัพท์',
    name = 'phone',
    count = 1,
    type = 'REMOVE',
    time = 8 * 1000,
    layout = 'bottomRight',
})
```

{% hint style="info" %}
ไอเทม เงิน และอาวุธที่เข้าออกผ่านอีเวนต์มาตรฐานของ ESX ถูกดักให้อัตโนมัติอยู่แล้ว เรียก `itemNotify` เองเฉพาะกรณีที่ไม่ได้ผ่านอีเวนต์เหล่านั้น เช่น กาชา หรือรางวัลที่ให้ตรง ๆ
{% endhint %}

ยิงหลายชิ้นติด ๆ กันได้เลย ระบบจะรวมให้เองในการ์ดใบเดียว

```lua
for _, it in ipairs(loot) do
    exports['Hyper_Notifyall']:itemNotify({
        action = 'Item',
        label = it.label,
        name = it.name,
        count = it.count,
        type = 'info',
        layout = 'bottomRight',
    })
end
```
