# Export

Hyper_Notifyall เปิดให้เรียก 28 export — 20 ตัวฝั่งผู้เล่น และ 8 ตัวฝั่งเซิร์ฟเวอร์ ทุกตัวเรียกได้จาก resource ไหนก็ได้โดยไม่ต้องประกาศ dependency

## จะทำอะไร ใช้ตัวไหน

| อยากทำ | ใช้ export | ฝั่ง | ตัวอย่างในเซิร์ฟ |
| --- | --- | --- | --- |
| เด้งข้อความบอกผู้เล่น | `sendNotify` | ผู้เล่น / เซิร์ฟเวอร์ | แทบทุก resource — 79 ตัว |
| แจ้งข้อความเข้าในโทรศัพท์ | `sendMessageNotify` | ผู้เล่น / เซิร์ฟเวอร์ | ระบบโทรศัพท์ |
| แจ้งว่ามีสายเรียกเข้า | `phoneNotify` + `phoneNotifyClose` | ผู้เล่น / เซิร์ฟเวอร์ | ระบบโทรศัพท์ |
| โชว์ไอเทมเข้า–ออกเอง | `itemNotify` | ผู้เล่น / เซิร์ฟเวอร์ | ระบบกระเป๋า กาชา รางวัล |
| ป้ายปุ่มกดตอนเข้าใกล้จุด | `showHelpNotify` + `hideHelpNotify` | ผู้เล่น | 33 resource — ร้านค้า อู่ซ่อม งานอาชีพ |
| ป้ายลอยกับพิกัดในโลก | `ShowTextUI3D` + `RemoveTextUI3D` | ผู้เล่น | งานส่งของ งานเก็บขยะ ตกปลา คราฟต์ |
| หลอดโหลดแถบยาว | `progressBar` + `stopProgressBar` | ผู้เล่น | 25 resource — งานอาชีพ ปล้น ซ่อมรถ |
| หลอดเพชรจับเวลา | `Progress` + `StopProgress` | ผู้เล่น | 11 resource — บัฟ คูลดาวน์ งานที่รอนาน |
| ประกาศกลางจอ | `announcementNotify` | ผู้เล่น / เซิร์ฟเวอร์ | อีเวนต์ ตกปลา ยึดธง |
| นับถอยหลังรีสตาร์ท/ลบรถ | `serverNotify` | ผู้เล่น | ปกติระบบสั่งเองตามตารางเวลา |
| การ์ดโปรไฟล์ผู้เล่น | `SendAlert` / `SendGlobalAlert` | ผู้เล่น / เซิร์ฟเวอร์ | แจ้งจับกุม แจ้งเข้าเซิร์ฟ |

## แยกตามหมวด

- [แจ้งเตือนและไอเทม](notify.md) — `sendNotify` `sendMessageNotify` `phoneNotify` `phoneNotifyClose` `itemNotify`
- [TextUI และหลอดโหลด](ui.md) — `showHelpNotify` `hideHelpNotify` `ShowTextUI3D` `RemoveTextUI3D` `HideTextUI3D` `progressBar` `stopProgressBar` `Progress` `StopProgress`
- [ประกาศและการ์ดโปรไฟล์](system.md) — `announcementNotify` `serverNotify` และ export การ์ดโปรไฟล์ทั้งหมด

## รายการทั้งหมด

### ฝั่งผู้เล่น (20)

| Export | หน้าที่ |
| --- | --- |
| `sendNotify` | การ์ดแจ้งเตือนทั่วไป |
| `sendMessageNotify` | การ์ดข้อความโทรศัพท์ |
| `phoneNotify` | การ์ดสายเรียกเข้า |
| `phoneNotifyClose` | ปิดการ์ดสายเรียกเข้า |
| `itemNotify` | การ์ดไอเทมเข้า–ออก |
| `serverNotify` | การ์ดนับถอยหลังรีสตาร์ท/ลบรถ |
| `showHelpNotify` | ป้ายปุ่มกดติดหน้าจอ |
| `hideHelpNotify` | ปิดป้ายปุ่มกด |
| `ShowTextUI3D` | ลงทะเบียนป้ายลอยตามพิกัด |
| `RemoveTextUI3D` | ลบป้ายลอยทีละจุด |
| `HideTextUI3D` | ล้างป้ายลอยทุกจุด |
| `progressBar` | หลอดโหลดแถบยาว |
| `stopProgressBar` | ยกเลิกหลอดโหลดแถบยาว |
| `Progress` | หลอดเพชรจับเวลา (มี callback) |
| `StopProgress` | ยกเลิกหลอดเพชร |
| `announcementNotify` | ประกาศกลางจอ |
| `alertNotify` | การ์ดโปรไฟล์ (ส่ง table เต็ม) |
| `ShowAlert` | การ์ดโปรไฟล์ (ตั้งชื่อและรูปเอง) |
| `SendAlert` | ยิงการ์ดโปรไฟล์ของตัวเองให้ทุกคน |
| `SendAlertToPlayer` | ยิงการ์ดโปรไฟล์ไปหาผู้เล่นคนเดียว |

### ฝั่งเซิร์ฟเวอร์ (8)

| Export | หน้าที่ |
| --- | --- |
| `sendNotify` | ส่งการ์ดแจ้งเตือนไปหาผู้เล่นที่ระบุใน `source` |
| `sendMessageNotify` | ส่งการ์ดข้อความ |
| `phoneNotify` | ส่งการ์ดสายเรียกเข้า |
| `itemNotify` | ส่งการ์ดไอเทม |
| `announcementNotify` | สั่งประกาศกลางจอ |
| `SendGlobalAlert` | การ์ดโปรไฟล์ถึงทุกคน |
| `SendPlayerAlert` | การ์ดโปรไฟล์ถึงผู้เล่นคนเดียว |
| `SendCustomAlert` | การ์ดโปรไฟล์แบบตั้งชื่อและรูปเอง ส่งถึงคนเดียวหรือหลายคน |

{% hint style="info" %}
export ฝั่งเซิร์ฟเวอร์ต้องใส่ `source = source` ลงไปในตารางที่ส่ง เพื่อบอกว่าจะส่งการ์ดไปหาผู้เล่นคนไหน
{% endhint %}

## เรียกผ่านอีเวนต์ก็ได้

ทุก export ฝั่งผู้เล่นมีอีเวนต์คู่กัน ใช้เมื่อไม่อยากผูกกับ export โดยตรง

```lua
TriggerEvent('Hyper_Notifyall:sendNotify', { ... })
TriggerEvent('Hyper_Notifyall:itemNotify', { ... })
TriggerEvent('Hyper_Notifyall:showHelpNotify', { ... })
```

ฝั่งเซิร์ฟเวอร์ยิงถึงผู้เล่นคนเดียวได้แบบนี้

```lua
TriggerClientEvent('Hyper_Notifyall:sendNotify', src, { ... })
TriggerClientEvent('Hyper_Notifyall:itemNotify', src, { ... })
```
