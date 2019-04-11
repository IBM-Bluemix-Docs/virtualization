---
copyright:
  years: 2014, 2018
lastupdated: "2018-11-14"

subcollection: virtualization

keywords: Avago SafeStore, encryption
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 使用 Avago SafeStore Encryption Services 啟用磁碟機安全
{: #enabling-drive-security-by-using-avago-safestore-encryption-services}

設定磁碟機安全有助於防止在沒有安全金鑰的情況下，存取已移除磁碟上儲存的資料。沒有此金鑰就無法回復磁碟機資料。{{site.data.keyword.BluSoftlayer_full}} 會在選取資料中心提供「自行加密磁碟機 (SED)」，作為可在裸機伺服器上購買的磁碟機。我們的美國資訊中心提供 10 TB SATA 磁碟機。

## 必要條件
{: #prerequisites-enabling-drive-security-by-using-avago-safestore-encryption-services}

* 裸機伺服器（含 SED 磁碟機）- 10 TB SATA
* LSI/AVAGO MegaRAID SAS 9361 -8i 或類似的 LSI/AVAGO RAID 卡
* 已安裝的 Mega RAID Storage Manager 軟體

## 使用 MegaRAID Storage Manager (MSM) 啟用磁帶機安全
{: #enabling-drive-security-by-using-megaraid-storage-manager-msm-}

您可以使用 MegaRAID Storage Manager，將安全金鑰及安全保護資料設定在其中。您也可以使用伺服器啟動時需要的 WebBIOS 介面，來輸入 MegaRAID 卡 BIOS 以配置磁碟機安全設定。如需「MegaRAID 控制器卡」SAS 9361-8i 的相關資訊，請參閱 Broadcom 網站 [MegaRAID SAS 9361-8i ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.broadcom.com/products/storage/raid-controllers/megaraid-sas-9361-8i#documentation)。

### 識別預先安裝的 SED 磁碟機
{: #identifying-preinstalled-sed-drives}

MegaRAID Storage Manager 預先安裝在大部分支援的作業系統上。如果沒有，您可以從 Broadcom 網站手動安裝它。如需相關資訊，請參閱 [MegaRAID SAS 9361-8i 下載](https://www.broadcom.com/products/storage/raid-controllers/megaraid-sas-9361-8i#downloads)。

您可以使用系統認證來開啟 MegaRAID Storage Manager。在此範例中，使用 Windows 機器，且已預先安裝 MSM。

啟動 MSM 時，您必須輸入**使用者名稱**及**密碼**，這是特許使用者（管理者）及密碼。

<!--![Figure 1](images/1_adapter_login.jpg)-->

按一下**實體**標籤。然後，按一下系統上可用的磁碟機。**內容**窗格具有**磁碟機安全內容**，提及**具備全磁碟加密功能**欄位，其會顯示**是**。在使用的範例中，會使用 2 顆非 SED 磁碟和 4 顆 SED 磁碟。

<!--![Figure 2](images/1_fde_capable_drives.jpg)-->

### 在控制器上啟用磁碟機安全
{: #enabling-drive-security-at-the-controller}

1. 若要啟用磁碟機安全，請從**實體**標籤中用滑鼠右鍵按一下**控制器 0：AVAGO MegaRAID SAS 9361-8i**，然後選取**啟用磁碟機安全**。
  * 您現在可以輸入**安全金鑰 ID** 及**安全金鑰**。如果您有多個安全金鑰，安全金鑰 ID 可協助您識別要使用的安全金鑰。您必須將安全金鑰記錄在安全位置中。重新配置磁碟機（例如，移除或重新插入磁碟機）時，需要安全金鑰。若沒有安全金鑰，就無法擷取磁區中儲存的任何資料，而此磁區是從 SED 建立的。無法擷取忘記的安全金鑰。也可以設定啟動時密碼，以在系統暫停時輸入這裡設定的密碼。啟動時密碼是選用的，如果已設定它，則每當系統重新啟動時，您就必須登入 IPMI，並鍵入啟動密碼。向下捲動並勾選指出**我已記錄安全設定以供未來參照**的方框，然後按一下**是**以啟用磁碟機安全。
  * 啟用磁碟機安全時，**控制器 0 AVAGO MegaRAID SAS 9361-8i** 會出現黃色金鑰影像。
2. 現在使用 SED 建立安全磁區。您可以從**邏輯**標籤中用滑鼠右鍵按一下**控制器 0**，然後選取  **建立虛擬磁碟機**。
3. 選擇**進階**選項。畫面需要指定 **RAID 層次**及**磁碟機安全方法**，作為**全磁碟加密 (FDE)**。選取需要的「FDE 磁碟機」，然後按一下**新增** > **建立磁碟機群組** > **下一步**。
4. 檢閱虛擬磁碟機設定，並進行任何必要的變更。**讀取原則**的建議設定是**一律先讀**。**寫入原則**的建議設定是**寫回**。按一下**建立虛擬磁碟機**。按一下**是**來接受由於 BBU 而造成的「寫回」原則影響。按**下一步**，並檢閱摘要畫面。按一下**完成**。
5. 若要確認已保護虛擬磁碟，請按一下**邏輯**標籤及已建立的虛擬磁碟機。您會在**磁碟機安全內容**中看到**安全**欄位標示為**是**。

<!--![Figure 3](images/2_enable_drive_security.jpg)-->
<!--![Figure 4](images/3_security_key_details_page.jpg)-->
<!--![Figure 5](images/4_security_key_set_0.jpg)-->
<!--![Figure 6](images/9_create_vd_with_fde_drives.jpg)-->
<!--![Figure 7](images/10_create_vd_advanced_select_raid_drive_encryption_0.jpg)-->
<!--![Figure 8](images/create_vd_settings.jpg)-->
<!--![Figure 9](images/6_vd_secured_confirmation_0.jpg)-->

如果伺服器隨附已使用 SED 磁碟機建立的 RAID 磁區，您可以遵循下列步驟來保護磁區的安全。

在**邏輯**標籤中，用滑鼠右鍵按一下**磁碟機群組**，然後選取**使用 FDE 保護安全**。如果您的磁區具有混合的 FDE 及非 FDE 磁碟機，則看不到此選項。

<!--![Figure 10](images/5_secure_existing_vd_with_fde_drives_0.jpg)-->

若要移除磁碟機安全，您必須先刪除安全的虛擬磁碟，並用滑鼠右鍵按一下「控制器 0」，以**停用磁碟機安全**。此功能會安全地消除其中的資料，並移除磁碟機安全。

您也可以設定磁碟機安全，方法為使用 webBIOS，並在啟動時透過 IPMI 登入，然後輸入 RAID BIOS。如需相關資訊，請參閱 **12 Gb/s MegaRAID SAS Software User Guide** 中的 **Avago SafeStore Encryption Services**。
