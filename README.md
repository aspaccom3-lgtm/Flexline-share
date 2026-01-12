<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Share Library Flex</title>
    <script charset="utf-8" src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <style>
        body {
            font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f8f9fa;
        }
        .container { text-align: center; }
        .loading-text { color: #555; font-size: 18px; }
    </style>
</head>
<body>

    <div class="container">
        <div class="loading-text">กำลังโหลดข้อมูล...</div>
    </div>

    <script>
        async function main() {
            // 1. ใส่ LIFF ID ที่ได้จาก LINE Developers
            await liff.init({ liffId: "YOUR_LIFF_ID" });

            if (!liff.isLoggedIn()) {
                // ถ้ายังไม่ได้ Login ให้เด้งหน้า Login ของ LINE
                liff.login();
            } else {
                // ถ้า Login แล้ว ให้เปิดหน้าแชร์ทันที
                sendShare();
            }
        }

        async function sendShare() {
            // 2. ตรวจสอบว่าเปิดสิทธิ์ shareTargetPicker ใน LINE Console หรือยัง
            if (liff.isApiAvailable('shareTargetPicker')) {
                try {
                    const result = await liff.shareTargetPicker([
                        {
                            "type": "flex",
                            "altText": "ห้องสมุดยินดีต้อนรับ",
                            "contents": 
                            // --- ส่วนของ JSON Flex Message ---
                            {
                              "type": "bubble",
                              "hero": {
                                "type": "image",
                                "url": "https://images.unsplash.com/photo-1524995997946-a1c2e315a42f?auto=format&fit=crop&w=1000&q=80",
                                "size": "full",
                                "aspectRatio": "20:13",
                                "aspectMode": "cover"
                              },
                              "body": {
                                "type": "box",
                                "layout": "vertical",
                                "contents": [
                                  { "type": "text", "text": "Library Online", "weight": "bold", "size": "xl", "color": "#1DB446" },
                                  { "type": "text", "text": "แนะนำหนังสือใหม่และกิจกรรมห้องสมุด", "size": "sm", "margin": "md", "color": "#666666" }
                                ]
                              },
                              "footer": {
                                "type": "box",
                                "layout": "vertical",
                                "contents": [
                                  {
                                    "type": "button",
                                    "action": { "type": "uri", "label": "เข้าสู่เว็บไซต์ห้องสมุด", "uri": "https://www.google.com" },
                                    "style": "primary",
                                    "color": "#1DB446"
                                  }
                                ]
                              }
                            }
                            // --- จบส่วนของ JSON ---
                        }
                    ]);

                    if (result) {
                        // เมื่อแชร์สำเร็จ หรือปิดหน้าต่างเลือกเพื่อน
                        liff.closeWindow();
                    } else {
                        // กรณีผู้ใช้กดกากบาทปิดหน้าแชร์
                        liff.closeWindow();
                    }
                } catch (error) {
                    console.error("Error:", error);
                    alert("เกิดข้อผิดพลาด กรุณาตรวจสอบการตั้งค่า Scopes ใน LINE Console");
                }
            } else {
                alert("ฟีเจอร์แชร์ไม่รองรับบนเบราว์เซอร์นี้");
            }
        }

        main();
    </script>
</body>
</html>
