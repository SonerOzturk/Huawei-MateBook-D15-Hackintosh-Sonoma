<div align="center">

# Huawei MateBook D15 · Hackintosh

**macOS Sonoma 14.8.9 | Intel Core i5-10210U | OpenCore**

![macOS](https://img.shields.io/badge/macOS-Sonoma%2014.8.9-000000?logo=apple)
![AirDrop](https://img.shields.io/badge/AirDrop-Working-2ea44f)
![WiFi](https://img.shields.io/badge/Intel%20Wi--Fi-Working-0071c5)
![AWDL](https://img.shields.io/badge/AWDL-Active-2ea44f)
![OpenCore](https://img.shields.io/badge/Bootloader-OpenCore-blue)

**🇬🇧 [English](#english) · 🇹🇷 [Türkçe](#türkçe)**

</div>

---

## English

A hardware-specific OpenCore configuration for the Huawei MateBook D15, featuring working AirDrop with Intel Wi-Fi on macOS Sonoma. AirDrop confirmed by the owner on November 24, 2026.

### 💻 Hardware

| Component | Configuration |
|---|---|
| Laptop | Huawei MateBook D15 |
| CPU | Intel Core i5-10210U (Comet Lake) |
| Graphics | Intel UHD Graphics (UHD 620 label in config) |
| Wi-Fi | Intel PCI 8086:02F0 |
| Bluetooth | Intel USB 8087:0AAA |
| SMBIOS | MacBookAir9,1 |
| macOS | Sonoma 14.8.9 (23J631) |
| Boot | OpenCore, existing Windows dual-boot EFI |

### ✨ Features

| Feature | Status |
|---|---|
| macOS boot | ✅ Working |
| AirDrop | ✅ Working |
| Handoff | ✅* Working |
| AWDL | ✅ Working |
| Wi-Fi / Bluetooth | ✅ Working |
| Graphics acceleration | ✅ Working |
| Audio | ✅ Working |
| Trackpad / keyboard | ✅ Working |
| Battery indicator | ✅ Working |
| USB port mapping | ✅ Working |
| Brightness control | ✅ Working |
| Sleep / wake | ✅ Working |
| Windows dual boot | 🟢 Configured |
| Fingerprint / Touch ID, HDMI, camera | ❔ Not verified |
| Universal Clipboard, AirPlay, Continuity Camera | ❔ Not verified |

*Handoff worked before the update; post-update retest recommended. Other configured features are marked working at the owner’s request, not independently tested.

### 🚀 AirDrop fix

| Driver | Previous | Working configuration |
|---|---|---|
| ForbiddenItlwm | 0.5.4 | 0.6.0 beta.4 Sonoma |
| BTLEAdvFixup | 0.9.1 | 0.9.4 |
| IntelBluetoothFirmware | 2.4.0 | 2.4.0 |
| BlueToolFixup | 2.7.2 | 2.7.2 |
| Lilu | 1.7.1 | 1.7.1 |

IntelBTPatcher is disabled. ForbiddenItlwm and BTLEAdvFixup are restricted to Darwin 23.4.0–23.99.99 in this configuration.

After updating, `awdl0` appeared, `ItlwmAWDLCreateRequests` changed from 0 to 1, `ITLAWDLCoexAddrApplied` changed to Yes, and `ItlwmAWDLSyncProbeStage` changed from `controller-not-ready` to `frame-built`. `ITLAWDLCoexTxSubmitted` reached 1073 in the diagnostic snapshot. The owner subsequently confirmed AirDrop works.

### Boot arguments (as tested)

```text
darkwake=8 lilucpu=10 -igfxblt itlwm_cc=FR -btlfxallowanyaddr -btlfxnvramcheck -btlfxboardid revpatch=cpuname -btleadv -itlawdlcoexprobe -itlawdliface -itlawdlnativeup -itlawdlcoexrx -itlawdlcoextx -itlawdlcoexmulti -itlawdlcoexdata -itlawdlmitx -itlawdlelicit -itlawdlcaps -itlawdlsvcmerge -itlawdlawsync itlawdlintcoal=4 -itlawdlcoexbatx -itlawdlcoexpeerpause -itlawdlcoexbatrace itlawdltxqueue=4096
```

These include experimental driver options and a France regulatory override (`itlwm_cc=FR`); do not assume they are appropriate in other regions. Check local regulations and upstream documentation before use.

### 🛠️ Installation

1. Back up your complete EFI partition and prepare a bootable recovery USB.
2. Generate your own unique MacBookAir9,1 SMBIOS values (SystemSerialNumber, MLB, SystemUUID, ROM) with GenSMBIOS. This public config intentionally has empty identifiers and a zero ROM. Do not boot or sign into Apple services with these placeholders.
3. Validate config.plist with the matching OpenCore ocvalidate utility. Review hardware differences, USB mapping and security settings. SecureBootModel is Disabled in this configuration.
4. This archive contains **EFI/OC only**. It is not a standalone bootable EFI. Obtain the matching OpenCore BOOTx64.efi from the official release for a fresh installation, or retain the matching existing bootloader for an update. The exact OpenCore version has not been verified.
5. Test from USB before replacing the internal OC folder. Preserve EFI/Microsoft and existing Windows boot files. Keep backups outside the EFI partition; the tested 104.9 MB partition could not hold both OC copies.
6. Reboot and test AirDrop in both directions and Handoff.

```sh
kmutil showloaded 2>/dev/null | grep -iE 'ForbiddenItlwm|BTLEAdvFixup'
ifconfig awdl0
ioreg -l -w0 | grep -E 'ItlwmAWDLSyncProbeStage|ItlwmAWDLInterfaceBuild|ItlwmAWDLCreateRequests|ITLAWDLCoexTxSubmitted|ITLAWDLCoexAddrApplied'
```

### 🙏 Credits and redistribution

- [OpenCore and Acidanthera projects](https://github.com/acidanthera)
- [OpenIntelWireless](https://github.com/OpenIntelWireless)
- [VoodooI2C](https://github.com/VoodooI2C)
- [USBToolBox](https://github.com/USBToolBox)
- ForbiddenItlwm / BTLEAdvFixup contributors and original MateBook configuration contributors.

Third-party binaries, themes and ACPI patches retain their respective authors' rights. Upstream provenance and redistribution licenses for every bundled file have not been verified. Before publishing binaries, verify their licenses, include required notices and source links, or replace them with upstream download instructions. No blanket license is asserted over third-party files.

This is a hardware-specific, experimental configuration, not a guarantee of compatibility with other MateBook variants or macOS releases. macOS is subject to Apple's license terms.

---

## Türkçe

Huawei MateBook D15 için Intel Wi-Fi üzerinden AirDrop çalıştırabilen, macOS Sonoma uyumlu OpenCore yapılandırması. AirDrop 24 Kasım 2026 tarihinde cihaz sahibi tarafından doğrulandı.

### 💻 Donanım

| Bileşen | Yapılandırma |
|---|---|
| Bilgisayar | Huawei MateBook D15 |
| İşlemci | Intel Core i5-10210U (Comet Lake) |
| Grafik | Intel UHD Graphics (config etiketi UHD 620) |
| Wi-Fi | Intel PCI 8086:02F0 |
| Bluetooth | Intel USB 8087:0AAA |
| SMBIOS | MacBookAir9,1 |
| macOS | Sonoma 14.8.9 (23J631) |
| Önyükleme | OpenCore, mevcut Windows çift açılış EFI bölümü |

### ✨ Özellikler

| Özellik | Durum |
|---|---|
| macOS açılışı | ✅ Çalışıyor |
| AirDrop | ✅ Çalışıyor |
| Handoff | ✅* Çalışıyor |
| AWDL | ✅ Çalışıyor |
| Wi-Fi / Bluetooth | ✅ Çalışıyor |
| Grafik hızlandırma | ✅ Çalışıyor |
| Ses | ✅ Çalışıyor |
| Trackpad / klavye | ✅ Çalışıyor |
| Pil göstergesi | ✅ Çalışıyor |
| USB portları | ✅ Çalışıyor |
| Parlaklık kontrolü | ✅ Çalışıyor |
| Uyku / uyanma | ✅ Çalışıyor |
| Windows çift işletim sistemi | 🟢 Yapılandırılmış |
| Parmak izi / Touch ID, HDMI, kamera | ❔ Doğrulanmadı |
| Evrensel Pano, AirPlay, Süreklilik Kamerası | ❔ Doğrulanmadı |

*Handoff güncelleme öncesinde çalışıyordu; güncelleme sonrası tekrar test önerilir. Diğer yapılandırılmış özellikler cihaz sahibinin isteğiyle çalışıyor olarak işaretlenmiştir; bağımsız olarak test edilmemiştir.

### 🚀 AirDrop çözümü

| Sürücü | Önceki | Çalışan sürüm |
|---|---|---|
| ForbiddenItlwm | 0.5.4 | 0.6.0 beta.4 Sonoma |
| BTLEAdvFixup | 0.9.1 | 0.9.4 |
| IntelBluetoothFirmware | 2.4.0 | 2.4.0 |
| BlueToolFixup | 2.7.2 | 2.7.2 |
| Lilu | 1.7.1 | 1.7.1 |

IntelBTPatcher devre dışıdır. ForbiddenItlwm ve BTLEAdvFixup Darwin 23.4.0–23.99.99 ile sınırlandırılmıştır.

| AWDL kontrolü | Önce | Sonra |
|---|---|---|
| awdl0 | Yok | ✅ UP / RUNNING, IPv6 |
| CreateRequests | 0 | 1 |
| AddrApplied | No | Yes |
| SyncProbeStage | controller-not-ready | frame-built |
| TxSubmitted | 0 | 1073 |

#### Test edilen boot-args

```text
darkwake=8 lilucpu=10 -igfxblt itlwm_cc=FR -btlfxallowanyaddr -btlfxnvramcheck -btlfxboardid revpatch=cpuname -btleadv -itlawdlcoexprobe -itlawdliface -itlawdlnativeup -itlawdlcoexrx -itlawdlcoextx -itlawdlcoexmulti -itlawdlcoexdata -itlawdlmitx -itlawdlelicit -itlawdlcaps -itlawdlsvcmerge -itlawdlawsync itlawdlintcoal=4 -itlawdlcoexbatx -itlawdlcoexpeerpause -itlawdlcoexbatrace itlawdltxqueue=4096
```

Deneysel sürücü seçenekleri ve Fransa bölge ayarı (`itlwm_cc=FR`) içerir. Yerel düzenlemeleri ve sürücü belgelerini kontrol edin.

### 🛠️ Kurulum

1. EFI bölümünün tamamını yedekleyin ve kurtarma USB'si hazırlayın.
2. GenSMBIOS ile kendinize ait MacBookAir9,1 SystemSerialNumber, MLB, SystemUUID ve ROM değerlerini üretin. Paylaşılan config boş kimlikler ve sıfır ROM içerir; bunlarla sistemi başlatmayın veya Apple servislerine giriş yapmayın.
3. Uyumlu OpenCore ocvalidate ile config.plist dosyasını doğrulayın. Donanım, USB eşlemesi ve güvenlik ayarlarını inceleyin. SecureBootModel devre dışıdır.
4. Paket yalnızca **EFI/OC** içerir; tek başına başlatılabilir tam EFI değildir. Yeni kurulum için eşleşen resmi OpenCore BOOTx64.efi dosyasını edinin. OpenCore sürümü doğrulanmamıştır.
5. Önce USB üzerinden deneyin. EFI/Microsoft ve Windows açılış dosyalarını koruyun. Yedekleri EFI dışında tutun; test edilen 104,9 MB bölüm iki OC kopyasını alamamıştır.
6. Yeniden başlatıp iki yönde AirDrop ve Handoff testi yapın.

```sh
kmutil showloaded 2>/dev/null | grep -iE 'ForbiddenItlwm|BTLEAdvFixup'
ifconfig awdl0
ioreg -l -w0 | grep -E 'ItlwmAWDLSyncProbeStage|ItlwmAWDLInterfaceBuild|ItlwmAWDLCreateRequests|ITLAWDLCoexTxSubmitted|ITLAWDLCoexAddrApplied'
```

### 🙏 Teşekkürler ve lisanslar

- [OpenCore / Acidanthera](https://github.com/acidanthera)
- [OpenIntelWireless](https://github.com/OpenIntelWireless)
- [VoodooI2C](https://github.com/VoodooI2C)
- [USBToolBox](https://github.com/USBToolBox)
- ForbiddenItlwm, BTLEAdvFixup ve orijinal MateBook yapılandırması geliştiricileri.

Üçüncü taraf dosyalarının hakları ilgili geliştiricilere aittir. Tüm dosyaların kaynakları ve dağıtım lisansları doğrulanmamıştır. Yayınlamadan önce lisansları kontrol edin, gerekli bildirimleri ve kaynak bağlantılarını ekleyin veya dosyalar yerine resmi indirme talimatlarını paylaşın. Diğer donanım ve macOS sürümleriyle uyumluluk garanti edilmez. macOS, Apple lisans koşullarına tabidir.
