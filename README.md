# BitBox01-Recovery
คู่มือการกู้ข้อมูล Bitcoin, Litecoin และ Ethereum จาก [Digital BitBox01](https://shiftcrypto.ch/bitbox01/) / BitBox01 Recovery Guide for Bitcoin, Litecoin and Ethereum

![BitBox01](/Pictures/bb01_hero_320.png)

## สิ่งที่ต้องเตรียม
1. ไฟล์ PDF จาก backup microSD
2. [Digital Bitbox Backup Center](https://api.github.com/repos/digitalbitbox/html_backup/zipball/v1.0.0) เมื่อ download เสร็จแล้วให้แตกไฟล์ไปไว้ที่ C:\BitBox01
3. [BIP32 Tool](https://github.com/iancoleman/bip39/releases/latest/download/bip39-standalone.html)

ตัวอย่างไฟล์ทั้งหมดที่ต้องใช้งาน

![BitBox01_folder](/Pictures/BitBox01_folder.png)

## คำแนะนำเพื่อความปลอดภัย

เพื่อป้องกันข้อมูล recovery key ของ BitBox01 เราจะทำการ recovery ทั้งหมดแบบ offline โดย

1. ปิดโปรแกรมทั้งหมด
2. ทำการถอดสาย LAN, ปิด Wifi เพื่อตัดการเชื่อมต่อจาก internet
3. เมื่อทำการ recovery ข้อมูลทั้งหมดเสร็จแล้ว แนะนำให้ทำการ restart Windows ก่อน จึงค่อยกลับมาใช้งานตามปกติ

## ขั้นตอนการกู้ข้อมูล Bitcoin, Litecoin และ Ethereum จาก Digital BitBox01

1. เปิดไฟล์ PDF จาก backup microSD ทำการ copy "Wallet backup" ไว้
2. เปิดไฟล์ "backup.html" ที่อยู่ใน folder "digitalbitbox-html_backup-89b35aa" จากนั้นให้
    - paste "Wallet backup" ในช่อง "wallet backup"
    - กรอก "recovery passowrd" ในช่อง "password"
    - กดปุ่ม "GENERATE" แล้วรอสักครู่

    ![backup_01](/Pictures/backup_01.png)

3. เราจะได้ "BIP32 extended master private key" ซึ่งเป็นข้อมูลที่สำคัญมาก **ห้ามให้ผู้อื่นทราบเด็ดขาด** เนื่องจากเป็นข้อมูลที่นำไปสร้าง wallet address, public key และ private key ทั้งหมดของ BitBox01 ได้

4. เปิดไฟล์ "bip39-standalone.html" จากนั้นให้
    - เลือก Coin ที่เราต้องการ ระหว่าง
        - BTC - Bitcoin
        - LTC - Litecoin สำหรับ Litecoin จะไม่สามารถกูข้อมูลได้ทันที ต้องทำการแก้ไขไฟล์ bip39-standalone.html ตามวิธีในห้วข้อ [การแก้ไขไฟล์ bip39-standalone.html ให้รองรับการกู้ข้อมูล Litecoin จาก Digital BitBox01](#user-content-การแก้ไขไฟล์-bip39-standalonehtml-ให้รองรับการกู้ข้อมูล-litecoin-จาก-digital-bitbox01) ก่อน จึงจะทำตามขั้นตอนต่อไปได้
        - ETH - Ethereum
    - paste "BIP32 extended master private key" ลงในช่อง "BIP32 Root Key" แล้วรอสักครู่ โปรแกรมจะคำนวนข้อมูลให้

    ![bip39_01](/Pictures/bip39_01.png)

5. เมื่อคำนวนเสร็จแล้ว ให้เลื่อนลงไปที่ Derivation Path โดยที่
    - ถ้าเลือก Coin เป็น BTC - Bitcoin ให้เลือก BIP49
    - ถ้าเลือก Coin เป็น LTC - Litecoin ให้เลือก BIP49 จากนั้นให้เอาเครื่องหมาย ✓ ออกจากหัวข้อ **Prefixes**
    - ถ้าเลือก Coin เป็น ETH - Ethereum ให้เลือก BIP44

6. ข้อมูล address, public key และ private key ของ BitBox01 จะปรากฏอยู่ที่ Derived Addresses
7. ตรรวจสอบ address จาก "BIP39 Tool" กับ "BitBox Wallet App" ว่าตรงกันหรือไม่ ถ้าตรงกันแสดงว่าข้อมูลทั้งหมดถูกต้อง
8. ทำการสำรองข้อมูล address, public key และ private key ของ BitBox01 ไปไว้ในที่ปลอดภัย
    - ไม่ควรเก็บข้อมูลเอาไว้บนคอมพิวเตอร์แบบ plain text
    - แนะนำให้เก็บข้อมูลเอาไว้ในโปรแกรมประเภท Password Manager อย่างเช่นโปรแกรม [KeePass](https://keepass.info/) เป็นต้น

## การแก้ไขไฟล์ bip39-standalone.html ให้รองรับการกู้ข้อมูล Litecoin จาก Digital BitBox01
[BIP39 Tool v0.3.12](https://github.com/iancoleman/bip39/tree/0.3.12) ไม่สามารถกู้ข้อมูล Litecoin จาก Digital BitBox01 ได้ทันที ต้องมีการแก้ไขไฟล์ bip39-standalone.html ก่อน โดยทำการเพิ่ม code ดังนี้

```
bitcoinjs.bitcoin.networks.litecoinXprv.p2wpkh = {
	baseNetwork: "litecoin",
	messagePrefix: '\x19Litecoin Signed Message:\n',
	bech32: 'ltc',
	bip32: {
		public: 0x0488b21e,
		private: 0x0488ade4
	},
	pubKeyHash: 0x30,
	scriptHash: 0x32,
	wif: 0xb0
};

bitcoinjs.bitcoin.networks.litecoinXprv.p2wpkhInP2sh = {
	baseNetwork: "litecoin",
	messagePrefix: '\x19Litecoin Signed Message:\n',
	bech32: 'ltc',
	bip32: {
		public: 0x0488b21e,
		private: 0x0488ade4
	},
	pubKeyHash: 0x30,
	scriptHash: 0x32,
	wif: 0xb0
};
```

![BitBox01](/Pictures/bip32_litecoin_modified.png)

Credit: [Issue with extended keys on Litecoin #96](https://github.com/iancoleman/bip39/issues/96#issuecomment-502194456)

## Version ของโปรแกรมต่างๆ ที่ใช้เขียนบทความ
- Microsoft Windows Version 10.0.18362.207 (64-bit)
- Google Chrome Version 75.0.3770.100 (Official Build) (64-bit)
- [BitBox Wallet App Version 4.9.0](https://shiftcrypto.ch/app/)
- [BitBox01 Firmware Version 6.1.1](https://shiftcrypto.ch/app/)
- [Digital Bitbox Backup Center v1.0.0](https://github.com/digitalbitbox/html_backup/tree/v1.0.0)
- [BIP39 Tool v0.3.12](https://github.com/iancoleman/bip39/tree/0.3.12)

## ข้อมูลเพิ่มเติม
- [BIP คืออะไร? และกระเป๋าฮาร์ดแวร์ทำการแบ็คอัพจากคำ 24 คำได้อย่างไร?](https://siambc.com/bip-คืออะไร/)
- [วิธีใช้งาน: การใช้ KeePassXC](https://ssd.eff.org/th/module/วิธีใช้งาน-การใช้-keepassxc)
