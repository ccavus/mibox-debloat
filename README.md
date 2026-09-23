# Xiaomi Mi Box S (MDZ-22-AB) Safe Debloat & Performance Optimization

[English](#english) • [Türkçe](#türkçe)

---

<a name="english"></a>
## English

A comprehensive, non-root performance optimization and debloat guide specifically tailored for the **Xiaomi Mi Box S (MDZ-22-AB - Amlogic S905X)**.

### Why Avoid Custom ROMs on Mi Box S?
Flashing custom ROMs (such as Slimbox or generic ATV/AOSP ports) requires the Amlogic USB Burning Tool and permanently breaks **Widevine L1** keys and **Netflix ESN** device provisioning. As a result, streaming platforms like Netflix and Amazon Prime Video are permanently downgraded to 540p (SD).

This guide eliminates OEM telemetry, stops background bloatware, and switches to a lightweight launcher **without losing 4K DRM playback, requiring root, or risking a hard-brick**.

---

### Prerequisites
1. **Xiaomi Mi Box S** connected to your local network.
2. A computer (Windows/macOS/Linux) or an Android phone connected to the same network.
3. Enable **Developer Options** and **USB Debugging**:
   - Go to **Settings > Device Preferences > About**.
   - Click on **Build** 7 times until Developer Mode is unlocked.
   - Go to **Settings > Device Preferences > Developer Options** and enable **USB Debugging**.

---

### Step 1: Open ADB Port 5555 (Network Handshake)

On Android TV 9 and 12, port `5555` is closed by default and cannot be opened via local terminal without root. Use one of these two verified workarounds:

* **Method A — Via Android Smartphone (No cable needed):**
  1. Install **Bugjaeger Mobile ADB** from Google Play on your Android phone.
  2. Open Bugjaeger, tap the plug icon, enter `<TV_IP_ADDRESS>:5555`, and click **Connect**.
  3. Accept the RSA authorization prompt on the TV screen using the remote.
  4. Disconnect Bugjaeger so the computer can connect.
* **Method B — One-time USB Cable Handshake:**
  1. Connect your PC to the Mi Box using a USB-A to USB-A cable.
  2. Authorize the prompt on the TV screen.
  3. Run `adb tcpip 5555` on your computer, then unplug the cable.

Connect from your computer:
```bash
adb connect <TV_IP_ADDRESS>:5555
adb devices

(Verify output displays <IP>:5555 device)

Step 2: Install Projectivy Launcher

Download the official APK (e.g., from APKMirror) as projectivy.apk into your terminal directory:
# 1. Install the APK
adb install -r projectivy.apk

# 2. Grant accessibility service to capture the remote's Home button
adb shell settings put secure enabled_accessibility_services com.spocky.projengmenu/com.spocky.projengmenu.services.AccessibilityService

# 3. Launch Projectivy on TV
adb shell monkey -p com.spocky.projengmenu -c android.intent.category.LAUNCHER 1

Step 3: Remove Verified Bloatware Packages

Run this one-liner to remove background telemetry, Xiaomi channels, and unused Google services:

adb shell pm uninstall -k --user 0 com.mitv.tvhome.michannel && \
adb shell pm uninstall -k --user 0 com.xiaomi.android.tvsetup.partnercustomizer && \
adb shell pm uninstall -k --user 0 com.mitv.videoplayer && \
adb shell pm uninstall -k --user 0 com.mitv.milinkservice && \
adb shell pm uninstall -k --user 0 com.google.android.videos && \
adb shell pm uninstall -k --user 0 com.google.android.play.games && \
adb shell pm uninstall -k --user 0 com.google.android.feedback && \
adb shell pm uninstall -k --user 0 com.android.printspooler

Step 4: Disable Heavy Stock Launchers

Important: Only disable the default launchers after Projectivy Launcher has been installed and tested.

adb shell pm disable-user --user 0 com.google.android.tvlauncher && \
adb shell pm disable-user --user 0 com.mitv.tvhome.atv

Step 5: Disable UI Animations & Reboot

Remove window transition delays to make UI navigation instantaneous:

adb shell settings put global window_animation_scale 0.0 && \
adb shell settings put global transition_animation_scale 0.0 && \
adb shell settings put global animator_duration_scale 0.0 && \
adb reboot

Step 6: Memory Usage Verification

Allow the system 1–2 minutes to settle after reboot, then inspect RAM usage:

adb connect <TV_IP_ADDRESS>:5555
adb shell dumpsys meminfo | grep -E "Free RAM:|Used RAM:"
# On Windows Command Prompt:
# adb shell dumpsys meminfo | findstr /C:"Free RAM:" /C:"Used RAM:"

Rollback / Recovery

If you ever need to restore any uninstalled package or re-enable the stock launchers:

# Re-enable stock Google & Xiaomi launchers
adb shell pm enable com.google.android.tvlauncher
adb shell pm enable com.mitv.tvhome.atv

# Restore any removed system package
adb shell cmd package install-existing <PACKAGE_NAME>
# Example: adb shell cmd package install-existing com.google.android.videos


Türkçe

Xiaomi Mi Box S (MDZ-22-AB - Amlogic S905X) modeli için test edilmiş, root gerektirmeyen güvenli debloat ve performans optimizasyon kılavuzu.

Neden Özel ROM Yerine Bu Yöntem?

Amlogic USB Burning Tool ile cihaza Slimbox veya AOSP tabanlı özel ROM'lar yüklemek mümkündür; ancak bu işlem cihazın Widevine L1 ve Netflix ESN lisans anahtarlarını siler. Bu durum Netflix, Prime Video gibi lisanslı platformların kalıcı olarak 540p (SD) çözünürlüğe düşmesine yol açar.
Bu rehber; 4K DRM lisanslarını kaybetmeden, cihazı rootlamadan ve brick riski taşımadan Xiaomi telemetrisini, arka plan bloatware'lerini temizler ve hafif bir arayüze geçiş sağlar.

Gereksinimler

1. Yerel ağa bağlı Xiaomi Mi Box S.
2. Aynı ağa bağlı bir bilgisayar (Windows/macOS/Linux) veya Android telefon.
3. Geliştirici Seçenekleri ve USB Hata Ayıklamanın açılması:
    - Ayarlar > Cihaz Tercihleri > Hakkında yolunu izleyin.
    - Yapı Numarası (Build) üzerine 7 kez basarak geliştirici modunu aktif edin.
    - Ayarlar > Geliştirici Seçenekleri menüsünden USB Hata Ayıklamayı açın.

Adım 1: ADB Portunu (5555) Açma ve Bağlantı
Android 9 ve 12 sürümlerinde 5555 TCP portu varsayılan olarak kapalıdır ve yerel terminalde root olmadan açılamaz. Aşağıdaki iki yöntemden birini uygulayın:

    - Yöntem A — Android Telefon Üzerinden (Kablo Gerektirmez):
        1. Telefonunuza Google Play'den Bugjaeger Mobile ADB uygulamasını indirin.
        2. Mi Box ile telefonun aynı Wi-Fi'da olduğundan emin olun.
        3. Bugjaeger üzerinde bağlantı simgesine basıp <MI_BOX_IP>:5555 yazarak bağlanın.
        4. TV ekranına gelen izin onayını kumandadan verin, ardından telefon uygulamasını kapatın.

    - Yöntem B — Tek Seferlik USB Kablo ile Tetikleme:
        1. Mi Box'ı çift taraflı USB kablo ile bilgisayara bağlayın.
        2. TV ekranındaki yetkilendirmeyi onaylayın.
        3. Bilgisayarda adb tcpip 5555 komutunu verip kabloyu çıkarın.
    Bilgisayar terminalinden bağlanın:

    adb connect <MI_BOX_IP_ADRESI>:5555
    adb devices

(Listede <IP>:5555 device ifadesi görülmelidir)

Adım 2: Projectivy Launcher Kurulumu

Resmi/güvenilir kaynaktan (örn. APKMirror) güncel APK dosyasını indirip terminal dizininde adını projectivy.apk olarak değiştirin:

# 1. APK dosyasını cihaza yükleyin
adb install -r projectivy.apk

# 2. Kumandadaki Home (Ev) butonunu yakalaması için erişilebilirlik iznini verin
adb shell settings put secure enabled_accessibility_services com.spocky.projengmenu/com.spocky.projengmenu.services.AccessibilityService

# 3. Uygulamayı TV ekranında başlatın
adb shell monkey -p com.spocky.projengmenu -c android.intent.category.LAUNCHER 1

Adım 3: Doğrulanmış Bloatware Paketlerini Temizleme

Arka plan telemetrisini, Xiaomi kanallarını ve kullanılmayan servisleri kullanıcı 0 seviyesinden kaldırmak için tek satırlık komutu çalıştırın:

adb shell pm uninstall -k --user 0 com.mitv.tvhome.michannel && \
adb shell pm uninstall -k --user 0 com.xiaomi.android.tvsetup.partnercustomizer && \
adb shell pm uninstall -k --user 0 com.mitv.videoplayer && \
adb shell pm uninstall -k --user 0 com.mitv.milinkservice && \
adb shell pm uninstall -k --user 0 com.google.android.videos && \
adb shell pm uninstall -k --user 0 com.google.android.play.games && \
adb shell pm uninstall -k --user 0 com.google.android.feedback && \
adb shell pm uninstall -k --user 0 com.android.printspooler

Adım 4: Ağır Stok Başlatıcıları Devre Dışı Bırakma

Önemli: Bu adımı yalnızca Projectivy Launcher'ı kurup çalıştığını doğruladıktan sonra uygulayın.

adb shell pm disable-user --user 0 com.google.android.tvlauncher && \
adb shell pm disable-user --user 0 com.mitv.tvhome.atv

Adım 5: Arayüz Animasyonlarını Kapatma ve Yeniden Başlatma

Arayüz gecikmesini sıfırlamak için animasyon sürelerini kapatıp cihazı yeniden başlatın:

adb shell settings put global window_animation_scale 0.0 && \
adb shell settings put global transition_animation_scale 0.0 && \
adb shell settings put global animator_duration_scale 0.0 && \
adb reboot

Adım 6: Boş RAM Doğrulaması

Cihaz yeniden başladıktan 1–2 dakika sonra bellek kullanımını kontrol edin:

adb connect <MI_BOX_IP_ADRESI>:5555
adb shell dumpsys meminfo | grep -E "Free RAM:|Used RAM:"
# Windows Komut İstemi (CMD) için:
# adb shell dumpsys meminfo | findstr /C:"Free RAM:" /C:"Used RAM:"

Geri Alma (Rollback)

Kaldırılan bir paketi geri yüklemek veya eski başlatıcıları tekrar açmak için:

# Stok başlatıcıları tekrar aktif etme
adb shell pm enable com.google.android.tvlauncher
adb shell pm enable com.mitv.tvhome.atv

# Kaldırılan sistem paketini geri yükleme
adb shell cmd package install-existing <PAKET_ADI>
# Örnek: adb shell cmd package install-existing com.google.android.videos
