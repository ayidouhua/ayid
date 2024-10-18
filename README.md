<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>阿義豆花臺南總店營業時間表</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 20px;
        }
        h2 {
            text-align: center;
            margin-bottom: 20px;
        }
        table {
            border-collapse: collapse;
            width: 100%;
            margin: 0 auto;
            background-color: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        th, td {
            border: 1px solid #000;
            padding: 10px;
            text-align: center;
        }
        th {
            background-color: #4CAF50;
            color: white;
        }
        @media (max-width: 600px) {
            th, td {
                padding: 8px;
                font-size: 14px;
            }
            h2 {
                font-size: 18px;
            }
        }
    </style>
</head>
<body>
    <h2>阿義豆花臺南總店即時營業時間表(專案計畫)</h2>
    <table>
        <thead>
            <tr>
                <th>星期</th>
                <th>營業時間</th>
                <th>即時狀態</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>星期一</td>
                <td>公休</td>
                <td id="statusMon">休息中...</td>
            </tr>
            <tr>
                <td>星期二</td>
                <td>09:30 - 12:30</td>
                <td id="statusTue">檢查中...</td>
            </tr>
            <tr>
                <td>星期三</td>
                <td>公休</td>
                <td id="statusWed">休息中...</td>
            </tr>
            <tr>
                <td>星期四</td>
                <td>公休</td>
                <td id="statusThu">休息中...</td>
            </tr>
            <tr>
                <td>星期五</td>
                <td>09:30 - 12:30</td>
                <td id="statusFri">檢查中...</td>
            </tr>
            <tr>
                <td>星期六</td>
                <td>09:30 - 12:30</td>
                <td id="statusSat">檢查中...</td>
            </tr>
            <tr>
                <td>星期日</td>
                <td>09:30 - 12:30</td>
                <td id="statusSun">檢查中</td>
            </tr>
        </tbody>
    </table>

    <script>
        const businessHours = {
            "Monday": { closed: true },
            "Tuesday": { open: "09:30", close: "12:30" },
            "Wednesday": { closed: true },
            "Thursday": { closed: true },
            "Friday": { open: "09:30", close: "12:30" },
            "Saturday": { open: "09:30", close: "12:30" },
            "Sunday": { open: "09:30", close: "12:30" }
        };

        function updateStatus() {
            const now = new Date();
            const dayNames = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
            const currentDay = dayNames[now.getDay()];
            const currentTime = now.toTimeString().slice(0, 5);
            let nextRefreshTime = null;

            for (const [day, hours] of Object.entries(businessHours)) {
                const statusElement = document.getElementById(`status${day.slice(0, 3)}`);

                if (hours.closed) {
                    statusElement.innerText = "休息中";
                } else {
                    if (currentDay === day) {
                        if (currentTime >= hours.open && currentTime < hours.close) {
                            statusElement.innerText = "營業中";
                        } else {
                            statusElement.innerText = "休息中";

                            const [closeHour, closeMinute] = hours.close.split(":").map(Number);
                            const closeTime = new Date(now.getFullYear(), now.getMonth(), now.getDate(), closeHour, closeMinute);

                            if (currentTime < hours.close) {
                                const timeToClose = closeTime.getTime() - now.getTime();
                                nextRefreshTime = Math.max(0, timeToClose);
                            } else {
                                const [openHour, openMinute] = hours.open.split(":").map(Number);
                                const nextOpenTime = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1, openHour, openMinute);
                                nextRefreshTime = nextOpenTime.getTime() - now.getTime();
                            }
                        }
                    } else {
                        statusElement.innerText = "休息中";
                    }
                }
            }

            if (nextRefreshTime !== null) {
                setTimeout(() => location.reload(), nextRefreshTime);
            }
        }

        updateStatus();
        setInterval(updateStatus, 60000);
    </script>
</body>
</html>
