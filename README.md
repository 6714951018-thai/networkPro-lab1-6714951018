# networkPro-lab1-6714951018

**ชื่อ-สกุล:** ธัญรัตน์ พูนทอง
**รหัสนักศึกษา:** 6714951018
**รายวิชา:** Network Programming (CH1: Python Network)

## โครงสร้างโปรเจกต์

```
networkPro-lab1-6714951018/
├── README.md
├── tcp_echo_server/
│   ├── echo_server.py
│   ├── echo_client.py
│   ├── Dockerfile.server
│   └── Dockerfile.client
├── udp_server_test/
│   ├── udp_server.py
│   ├── udp_client.py
│   ├── Dockerfile.server
│   └── Dockerfile.client
└── screenshots/
    ├── tcp_result.png
    └── udp_result.png
```

## วิธีรันด้วย Python

เปิด 2 terminal ช่องแรกรัน server ช่องที่สองรัน client พิมพ์ `exit` เพื่อออกจาก client

**TCP**
```
cd tcp_echo_server
python echo_server.py
python echo_client.py
```

**UDP**
```
cd udp_server_test
python udp_server.py
python udp_client.py
```

## วิธีรันด้วย Docker

สร้างเครือข่ายสำหรับให้ container คุยกัน (ทำครั้งเดียว)
```
docker network create echo_network
```

**TCP**
```
cd tcp_echo_server
docker build -f Dockerfile.server -t echo_server .
docker build -f Dockerfile.client -t echo_client .
docker run -d --name echo_server --network echo_network echo_server
docker run -it --rm --name echo_client --network echo_network echo_client
```

**UDP**
```
cd udp_server_test
docker build -f Dockerfile.server -t udp_server .
docker build -f Dockerfile.client -t udp_client .
docker run -d --name udp_server --network echo_network udp_server
docker run -it --rm --name udp_client --network echo_network udp_client
```

เมื่อทดสอบเสร็จ ลบ container และเครือข่าย
```
docker rm -f echo_server udp_server
docker network rm echo_network
```

## การออกแบบและเหตุผลที่เลือกเทคนิค

- **Python `socket`**: เป็นโมดูลมาตรฐาน ไม่ต้องติดตั้งเพิ่ม และเห็นการทำงานของโปรโตคอลชัดเจน
- **TCP (`SOCK_STREAM`)**: เชื่อมต่อแบบ connection-oriented ข้อมูลครบและเรียงลำดับ เหมาะกับ echo ที่ต้องการความถูกต้อง
- **UDP (`SOCK_DGRAM`)**: ไม่ต้องสร้างการเชื่อมต่อ เร็วและเบา แต่ไม่รับประกันว่าข้อมูลจะถึง
- **Docker**: แยก server และ client เป็นคนละ image ให้รันได้เหมือนกันทุกเครื่อง และให้ client หา server ผ่านชื่อ container บนเครือข่ายเดียวกัน

## ผลการทดสอบ

### TCP

![TCP result](screenshots/tcp_result.png)

### UDP

![UDP result](screenshots/udp_result.png)

## แหล่งอ้างอิง

- Python documentation, socket: https://docs.python.org/3/library/socket.html
- Docker documentation: https://docs.docker.com/

