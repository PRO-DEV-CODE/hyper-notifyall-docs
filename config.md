# การตั้งค่า

## ไฟล์ config

| ไฟล์ | โหลดฝั่ง | ดูแลอะไร |
| --- | --- | --- |
| `config/config.general.lua` | ทั้งสองฝั่ง | ธีมสี ประกาศ การ์ดแจ้งเตือน ไอเทม รีสตาร์ท ลบรถ เลขนำโชค หลอดโหลด การ์ดโปรไฟล์ |
| `config/config.textui.lua` | ทั้งสองฝั่ง | ป้าย TextUI ลอยในโลก — ชนิดป้าย พิกัด และชื่อ object |
| `config/config.function.lua` | ทั้งสองฝั่ง | สะพานต่อเฟรมเวิร์ก ESX (อ่านกระเป๋า อาชีพ บัญชีเงิน) |
| `config/config.server.lua` | เซิร์ฟเวอร์เท่านั้น | Discord webhook ชื่อเซิร์ฟ IP รูปประกอบ ข้อความ |

{% hint style="warning" %}
`config.server.lua` ถูกโหลดเฉพาะฝั่งเซิร์ฟเวอร์ตามที่ตั้งไว้ใน `fxmanifest.lua` ค่าในไฟล์นี้จึงไม่ถูกส่งไปหาผู้เล่น — อย่าย้าย URL webhook ไปไว้ไฟล์อื่น
{% endhint %}

## ค่าที่ปรับบ่อย

### ธีมสี

```lua
-- config/config.general.lua
Config.Theme = 'seagreen'
-- seagreen (ค่าเริ่มต้น) | ember | azure | violet | crimson | amber | slate
```

### ตารางรีสตาร์ทและลบรถ

```lua
-- config/config.general.lua
Config.Default_Server = {
    enable = true,
    times = {
        { time = '06:00', duration = 0 },   -- duration 0 = รีทันที ไม่ขึ้นการ์ด
        { time = '12:00', duration = 0 },
        { time = '18:00', duration = 0 },
        { time = '00:00', duration = 0 },
    },
    command = {
        cmd = 'reserver',
        permissions = { ['owner'] = true, ['admin'] = true, ['superadmin'] = true },
    },
}

Config.Default_Vehicle = {
    enable = true,
    times = {
        { time = '05:55', duration = 5 },   -- นับถอยหลัง 5 นาทีก่อนลบ
        { time = '11:55', duration = 5 },
        { time = '17:55', duration = 5 },
        { time = '23:55', duration = 5 },
    },
}
```

### เลขนำโชค

```lua
-- config/config.general.lua
Config.LuckyNumber = {
    enable = true,
    count  = 3,      -- สุ่มกี่เลข
    min    = 5,      -- ID ต่ำสุด
    max    = 11,     -- ID สูงสุด
    rewards = {
        items = {
            { name = 'exp', count = 10 },
        },
        money = { cash = 50000, bank = 100000, black_money = 0 },
    },
}
```

### การ์ดไอเทม

```lua
-- config/config.general.lua
Config.Default_Inventory = {
    ShowMaxUICount = 5,                              -- การ์ดลิสต์โชว์ได้กี่แถว
    InventoryPath  = 'Hyper_Inventory/html/img/items',
    Item = {
        enable = true,
        time   = 8 * 1000,
        layout = 'bottomRight',
        sound  = { enable = true, name = 'notify-items.mp3', volume = 0.5 },
    },
    -- Account และ Weapon ตั้งแยกได้แบบเดียวกัน
}
```

### หัวข้อประกาศ

```lua
-- config/config.general.lua
Config.AnnounceLimit = 5        -- ประกาศพร้อมกันได้ไม่เกินกี่ข้อความ

Config.Default_Admin = {
    toggle = {
        command = 'openMenuAnm',   -- คำสั่งเปิดเมนูประกาศ
        key     = '',              -- ใส่ชื่อปุ่มเพื่อผูกคีย์ลัด
        group   = 'admin',         -- กลุ่มที่เปิดเมนูได้
    },
    role = {
        {
            name   = 'Admin',
            layout = 'topCenter',
            time   = 25 * 1000,
            custom = { title_announce = 'ประกาศแอดมิน' },
        },
    },
}
```

{% hint style="info" %}
พาธรูปใน `custom.logo` และ `custom.icon` ถูกอ่านจาก stylesheet ที่อยู่ใน `html/css/` จึงต้องขึ้นต้นด้วย `../img/` ไม่ใช่ `./img/` ถ้าใส่ผิดไอคอนจะหายไปแต่การ์ดยังขึ้นปกติ
{% endhint %}

### ป้าย TextUI ลอยในโลก

```lua
-- config/config.textui.lua
Config.TextUIOnGame = {
    enable = true,
    smooth = 15,    -- ความนุ่มของการขยับป้าย
    radian = 3,     -- ระยะที่เริ่มเห็นป้าย
    type = {
        ['info']  = { height = 0.3, custom = { icon = './img/icons/info-new.png', scale = 0.75 } },
        ['error'] = { height = 0.3, custom = { icon = './img/icons/error.png',    scale = 0.75 } },
    },
    location = {
        {
            title = 'ELECTRICAL',
            type = 'info',
            highlight = 'E',
            message = 'Press %s to repair electric',
            coords = {
                -- { x = -276.92, y = -773.96, z = 53.28 },
            },
        },
    },
}
```

### Discord

```lua
-- config/config.server.lua
ServerConfig.Discord = {
    webhook    = '',                 -- เว้นว่าง = ไม่ส่ง
    username   = 'ระบบแจ้งเตือนอัตโนมัติ',
    nameserver = 'ชื่อเซิร์ฟเวอร์',
    ipserver   = '127.0.0.1:30120',
}
```

### การ์ดโปรไฟล์

```lua
-- config/config.general.lua
Config.Default_Alert = {
    profile        = 'Discord',               -- 'Steam' | 'Discord' | 'None'
    defaultName    = 'SYSTEM',
    defaultProfile = 'img/alert-profile.png',
    time           = 5000,
}
```

ต้องมี convar สองตัวนี้ใน `server.cfg` ถ้าจะให้ดึงรูปโปรไฟล์จริง

```
set discord_token "BOT_TOKEN"
set steam_webApiKey "STEAM_KEY"
```

## สีและหน้าตา

| ไฟล์ | หน้าที่ |
| --- | --- |
| `html/css/palette.css` | ชุดสีกลาง — token หลักและบล็อกธีมทั้ง 6 แก้ที่เดียวเปลี่ยนทั้ง UI |
| `html/css/style.css` | โครงสร้างและขนาดของทุกการ์ด |
| `html/css/custom.css` | ชั้นเชื่อม — แปลงตัวแปรของ `style.css` ให้ไปอ่าน token จาก `palette.css` |
| `html/css/status.css` | HUD แถวสถานะ |

{% hint style="warning" %}
แก้สีที่ `palette.css` เท่านั้น การแก้สีลงใน `style.css` ตรง ๆ จะทำให้การสลับธีมไม่มีผลกับจุดนั้น
{% endhint %}
