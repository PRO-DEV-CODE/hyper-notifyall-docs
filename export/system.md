# ประกาศและการ์ดโปรไฟล์

## announcementNotify

ประกาศกลางจอ

![ประกาศกลางจอ](../.gitbook/assets/07_announcement.png)

```lua
exports['Hyper_Notifyall']:announcementNotify({
    text = 'เปิดกิจกรรมแข่งรถคืนนี้ 21:00 น.',
    amount = 1,
    role = 1,
})
```

| ค่า | ชนิด | ความหมาย |
| --- | --- | --- |
| `text` | string | ข้อความประกาศ |
| `amount` | number | ประกาศซ้ำกี่ครั้ง (ห่างกันครั้งละ 1 วินาที) |
| `role` | number | ลำดับหัวข้อประกาศใน `Config.Default_Admin.role` เริ่มที่ 1 |
| `source` | number | ใส่เมื่อเรียกจากฝั่งเซิร์ฟเวอร์ |

หัวข้อประกาศที่มีมาให้

| ลำดับ | ชื่อ | หัวข้อที่โชว์ |
| --- | --- | --- |
| 1 | Admin | ประกาศแอดมิน |
| 2 | System | ประกาศแอดมิน |
| 3 | Staff | ทีมงาน |
| 4 | Police | ทีมงาน |
| 5 | Council | ทีมงาน |
| 6 | medic | ประกาศแอดมิน |
| 7 | event | ประกาศอีเว้น |

{% hint style="warning" %}
ประกาศพร้อมกันได้ไม่เกิน 5 ข้อความ (`Config.AnnounceLimit`) เกินแล้วระบบจะขึ้นการ์ดเตือนแทนและไม่ประกาศให้
{% endhint %}

### เมนูประกาศของแอดมิน

![เมนูประกาศ](../.gitbook/assets/08_announce_menu.png)

แอดมินพิมพ์ประกาศเองได้โดยไม่ต้องเขียนโค้ด เปิดด้วยคำสั่ง `openMenuAnm` เฉพาะผู้เล่นกลุ่ม `admin` กดไอคอนเฟืองเพื่อเลือกหัวข้อ แล้วกด ESC เพื่อปิด

## serverNotify

การ์ดนับถอยหลังรีสตาร์ทและลบรถ

![การ์ดนับถอยหลัง](../.gitbook/assets/09_restart_countdown.png)

```lua
exports['Hyper_Notifyall']:serverNotify('restart', 300, 300)
exports['Hyper_Notifyall']:serverNotify('delete', 180, 180)
```

| พารามิเตอร์ | ชนิด | ความหมาย |
| --- | --- | --- |
| 1 | string | `'restart'` หรือ `'delete'` |
| 2 | number | วินาทีที่เหลือ |
| 3 | number | วินาทีตั้งต้น (ไม่ใส่ = เท่ากับค่าที่ 2) |

{% hint style="warning" %}
export ตัวนี้แค่ **แสดงการ์ด** เฉย ๆ ไม่ได้สั่งรีสตาร์ทหรือลบรถจริง การลงมือจริงอยู่ฝั่งเซิร์ฟเวอร์ทั้งหมด สั่งผ่านคำสั่ง `reserver` / `delvehicle` หรือปล่อยให้ตารางเวลาทำเอง
{% endhint %}

## การ์ดโปรไฟล์

![การ์ดโปรไฟล์](../.gitbook/assets/15_profile_alert.png)

### ฝั่งผู้เล่น

```lua
-- ยิงการ์ดของตัวเองให้ทุกคนเห็น (ดึงรูปโปรไฟล์ของผู้เรียกให้เอง)
exports['Hyper_Notifyall']:SendAlert('ถูกจับกุม', 'ข้อหา: ปล้นทรัพย์ · 45 เดือน')

-- ยิงไปหาผู้เล่นคนเดียว
exports['Hyper_Notifyall']:SendAlertToPlayer(targetId, 'ถูกจับกุม', 'ข้อหา: ปล้นทรัพย์')

-- แสดงการ์ดบนจอตัวเองโดยตั้งชื่อและรูปเอง
exports['Hyper_Notifyall']:ShowAlert('ระบบ', 'img/alert-profile.png', 'SYSTEM', 'เซิร์ฟจะรีใน 5 นาที')
```

### ฝั่งเซิร์ฟเวอร์

```lua
-- ถึงทุกคน
exports['Hyper_Notifyall']:SendGlobalAlert('ถูกจับกุม', 'ข้อหา: ปล้นทรัพย์', playerSrc)

-- ถึงผู้เล่นคนเดียว
exports['Hyper_Notifyall']:SendPlayerAlert(targetId, 'แจ้งเตือน', 'คุณถูกเรียกพบ', senderSrc)

-- ตั้งชื่อและรูปเอง ส่งถึงคนเดียวหรือหลายคน
exports['Hyper_Notifyall']:SendCustomAlert(-1, 'ระบบ', 'เซิร์ฟจะรีใน 5 นาที', 'SYSTEM', 'img/alert-profile.png')
exports['Hyper_Notifyall']:SendCustomAlert({ 1, 2, 3 }, 'หัวข้อ', 'รายละเอียด')
```

| Export | พารามิเตอร์ | คืนค่า |
| --- | --- | --- |
| `SendGlobalAlert` | `title, subtitle, playerSrc?` | `true` เมื่อส่งสำเร็จ |
| `SendPlayerAlert` | `targetPlayer, title, subtitle, senderPlayer?` | `false` ถ้าไม่พบผู้เล่นปลายทาง |
| `SendCustomAlert` | `targets, title, subtitle, customName?, customProfile?` | `targets` เป็นเลขเดียว `-1` หรือ table ก็ได้ |

{% hint style="info" %}
รูปโปรไฟล์ดึงจาก Discord หรือ Steam ตาม `Config.Default_Alert.profile` โดยอ่าน token จาก convar `discord_token` และ `steam_webApiKey` ใน `server.cfg` ถ้าดึงไม่ได้จะใช้รูปเริ่มต้นแทนและการ์ดยังขึ้นปกติ
{% endhint %}
