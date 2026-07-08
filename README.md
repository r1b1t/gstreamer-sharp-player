# GStreamer Sharp Player

C# WinForms üzerinde **GStreamer** entegrasyonunu gösteren basit ve genişletilebilir bir medya oynatıcı. Kullanıcının elle girdiği GStreamer pipeline'ını çalıştırır ve video çıktısını **VideoOverlay API** ile doğrudan WinForms paneline çizer.

RTSP / UDP / kamera yayını testleri, prototip geliştirme ve GStreamer'ı .NET tarafında öğrenmek için tasarlanmıştır.

## Özellikler

- 🎬 **Serbest pipeline girişi** — `gst-launch` sözdizimindeki herhangi bir pipeline'ı textbox'a yazıp çalıştırabilirsiniz
- 🖥️ **Panel içine render** — video, ayrı bir pencere yerine `VideoOverlayAdapter` ile formun içindeki panele çizilir
- ▶️ **Play / Stop kontrolü** — yeni pipeline başlatmadan önce eskisi güvenle sonlandırılır
- 🧹 **Doğru kaynak yönetimi** — pipeline ve bus nesneleri `Dispose` edilerek bellek sızıntısı önlenir; form kapanırken otomatik temizlik yapılır
- ⚠️ **Hata yakalama** — geçersiz pipeline veya eksik video sink durumunda kullanıcıya anlaşılır mesaj gösterilir

## Gereksinimler

- Windows + .NET Framework 4.8.1
- Visual Studio 2022
- [GStreamer](https://gstreamer.freedesktop.org/download/) runtime (**Complete** kurulum, MSVC x86_64)
- [GstSharp](https://www.nuget.org/packages/GstSharp) NuGet paketi (1.18.0)

## Kurulum

1. GStreamer'ın **Complete** sürümünü kurun ve `bin` klasörünü PATH'e ekleyin:

   ```
   C:\gstreamer\1.0\msvc_x86_64\bin
   ```

2. Repoyu klonlayın ve Visual Studio ile `gstreamerplayer.sln` dosyasını açın:

   ```bash
   git clone https://github.com/r1b1t/gstreamer-sharp-player.git
   ```

3. NuGet paketlerini geri yükleyin (Visual Studio genellikle otomatik yapar):

   ```
   Sağ tık (Solution) → Restore NuGet Packages
   ```

4. Derleyip çalıştırın.

## Kullanım

1. Pipeline'ı textbox'a yazın
2. **Play** ile başlatın — video panel üzerinde oynar
3. **Stop** ile sonlandırın

### Örnek Pipeline'lar

Test görüntüsü:

```
videotestsrc ! videoconvert ! d3dvideosink
```

Kamera yayını:

```
ksvideosrc ! videoconvert ! d3dvideosink
```

RTSP akışı:

```
rtspsrc location=rtsp://<adres> latency=100 ! decodebin ! videoconvert ! d3dvideosink
```

UDP (H.264) akışı:

```
udpsrc port=5000 caps="application/x-rtp" ! rtph264depay ! avdec_h264 ! videoconvert ! d3dvideosink
```

> **Not:** Videonun panel içine çizilebilmesi için pipeline'da `VideoOverlay` destekleyen bir sink (ör. `d3dvideosink`) bulunmalıdır.

## Proje Yapısı

```
├── Program.cs               # Uygulama girişi
├── Form1.cs                 # Pipeline başlatma/durdurma ve VideoOverlay mantığı
├── Form1.Designer.cs        # WinForms tasarım kodu
├── gstreamerplayer.sln      # Visual Studio çözümü
├── gstreamerplayer.csproj
└── packages.config          # NuGet bağımlılıkları (GstSharp 1.18.0)
```

## Kullanılan Teknolojiler

| Teknoloji | Amaç |
|---|---|
| C# WinForms (.NET Framework 4.8.1) | Masaüstü arayüzü |
| [GstSharp](https://www.nuget.org/packages/GstSharp) | GStreamer'ın .NET binding'i |
| GStreamer VideoOverlay API | Videoyu form paneline çizme |
| Visual Studio 2022 | Geliştirme ortamı |
