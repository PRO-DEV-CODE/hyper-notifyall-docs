# TextUI และหลอดโหลด

## showHelpNotify

ป้ายปุ่มกดติดหน้าจอ ใช้ใน 33 resource

![ป้ายปุ่มกดและหลอดโหลด](../.gitbook/assets/11_help_and_progress.png)

```lua
exports['Hyper_Notifyall']:showHelpNotify({
    key = 'E',
    message = 'กดเพื่อเริ่มซ่อมตู้ไฟฟ้า',
})
```

| ค่า | ชนิด | ค่าเริ่มต้น | ความหมาย |
| --- | --- | --- | --- |
| `key` | string | — | ปุ่มที่โชว์ในกรอบ |
| `message` | string | — | คำแนะนำ |
| `layout` | string | `'bottomCenter'` | มุมจอ |
| `sound` | table | จาก config | เสียงตอนป้ายขึ้น |

ป้ายค้างอยู่จนกว่าจะสั่งปิด เรียกซ้ำด้วยข้อความใหม่จะเป็นการแก้ข้อความบนป้ายเดิม ไม่ได้สร้างป้ายใหม่

## hideHelpNotify

```lua
exports['Hyper_Notifyall']:hideHelpNotify()
```

{% hint style="warning" %}
ป้ายนี้ไม่หายเอง ต้องเรียก `hideHelpNotify()` ตอนผู้เล่นเดินออกจากจุด ไม่งั้นป้ายค้างบนจอ
{% endhint %}

รูปแบบที่ใช้กันทั่วไป

```lua
local shown = false
CreateThread(function()
    while true do
        local sleep = 1000
        local ped = PlayerPedId()
        local dist = #(GetEntityCoords(ped) - targetCoords)

        if dist < 2.0 then
            sleep = 0
            if not shown then
                shown = true
                exports['Hyper_Notifyall']:showHelpNotify({ key = 'E', message = 'กดเพื่อใช้งาน' })
            end
            if IsControlJustReleased(0, 38) then
                -- ทำงาน
            end
        elseif shown then
            shown = false
            exports['Hyper_Notifyall']:hideHelpNotify()
        end

        Wait(sleep)
    end
end)
```

## ShowTextUI3D

ป้ายลอยกับพิกัดในโลก ลงทะเบียนครั้งเดียวแล้วป้ายขึ้นเองเมื่อผู้เล่นเข้าใกล้

![TextUI 3D](../.gitbook/assets/14_textui_3d.png)

```lua
exports['Hyper_Notifyall']:ShowTextUI3D({
    title = 'ELECTRICAL',
    type = 'info',
    highlight = 'E',
    message = 'Press %s to repair electric',
    coords = { x = coords.x, y = coords.y, z = coords.z },
})
```

| ค่า | ชนิด | ความหมาย |
| --- | --- | --- |
| `title` | string | หัวข้อบรรทัดบน |
| `type` | string | ชนิดป้ายจาก `Config.TextUIOnGame.type` — มาพร้อม `info` และ `error` |
| `highlight` | string | ปุ่มที่ไฮไลต์ แทนที่ `%s` ใน `message` |
| `message` | string | คำแนะนำบรรทัดล่าง |
| `coords` | table | พิกัดที่จะให้ป้ายลอยอยู่ |

ลงทะเบียนพิกัดเดิมซ้ำจะถูกข้ามให้เอง เรียกวนในลูปได้โดยไม่ซ้อน

## RemoveTextUI3D

ลบป้ายเฉพาะจุดของตัวเอง ส่งพิกัดชุดเดียวกับตอน `ShowTextUI3D`

```lua
exports['Hyper_Notifyall']:RemoveTextUI3D({ x = coords.x, y = coords.y, z = coords.z })
```

คืนค่า `true` เมื่อเจอและลบแล้ว คืน `false` เมื่อไม่มีจุดนั้นอยู่

## HideTextUI3D

```lua
exports['Hyper_Notifyall']:HideTextUI3D()
```

{% hint style="warning" %}
`HideTextUI3D()` ล้างป้ายของทุกสคริปต์ในเซิร์ฟ ไม่ใช่แค่ของตัวเอง ถ้าจะลบแค่จุดของตัวเองให้ใช้ `RemoveTextUI3D(coords)` แทน
{% endhint %}

## progressBar

หลอดโหลดแถบยาว ใช้ใน 25 resource

```lua
exports['Hyper_Notifyall']:progressBar({
    time = 15 * 1000,
    canCancel = false,
})
```

| ค่า | ชนิด | ค่าเริ่มต้น | ความหมาย |
| --- | --- | --- | --- |
| `time` | number | — | ระยะเวลา (มิลลิวินาที) |
| `canCancel` | boolean | `false` | `true` = กด Backspace ยกเลิกได้ |
| `layout` | string | `'bottomCenter'` | มุมจอ |
| `sound` | table | จาก config | เสียงตอนเริ่ม |

## stopProgressBar

```lua
exports['Hyper_Notifyall']:stopProgressBar()
```

หลอดจะเปลี่ยนเป็นไอคอนกากบาทแล้วค่อยหายไป ใช้ตอนงานถูกขัดจังหวะกลางคัน

## Progress

หลอดเพชรจับเวลา ซ้อนกันได้หลายอัน และมี callback ตอนครบเวลา

![หลอดเพชรจับเวลา](../.gitbook/assets/12_hyper_progress.png)

```lua
exports['Hyper_Notifyall']:Progress({
    icon = 'armor',
    time = 10000,
    color = '#e74c3c',
    X = 'center',
    Y = 'end',
}, function()
    print('ครบเวลาแล้ว')
end)
```

| ค่า | ชนิด | ค่าเริ่มต้น | ความหมาย |
| --- | --- | --- | --- |
| `icon` | string | — | ชื่อไอเทม (หารูปจากโฟลเดอร์ระบบกระเป๋าให้เอง) หรือใส่ URL เต็มก็ได้ |
| `time` | number | — | ระยะเวลา (มิลลิวินาที) |
| `color` | string | `'#56b9fc'` | สีวงแหวน |
| `X` | string | `'center'` | `left` `center` `right` |
| `Y` | string | `'end'` | `start` `center` `end` |
| `scriptname` | string | ชื่อ resource ที่เรียก | ใช้แยกหลอดเวลาจะสั่งหยุด |
| callback | function | — | ทำงานเมื่อครบเวลา |

## StopProgress

```lua
exports['Hyper_Notifyall']:StopProgress()
exports['Hyper_Notifyall']:StopProgress('ชื่อที่ตั้งไว้ใน scriptname')
```

ไม่ใส่ชื่อ = หยุดหลอดของ resource ที่เรียก

{% hint style="info" %}
หลอดเพชรถูกล้างให้เองเมื่อ resource เจ้าของหยุดทำงาน ไม่ต้องห่วงหลอดค้างตอน restart สคริปต์
{% endhint %}
