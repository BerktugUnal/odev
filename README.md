# odev
import java.util.Random;//rastgele şehir belirlemek için
import java.util.Scanner;

public class AdamAsmaca {

    public static void main(String[] args) {
        // Oyunun soracağı şehirler
        String[] sehirler = {"ankara", "istanbul", "izmir", "bursa", "adana", "antalya"};

        // Rastgele şehir seçmek için
        Random random = new Random();
        String secilenSehir = sehirler[random.nextInt(sehirler.length)];

        // Tahmin edilen kelimenin '_' ile gösterilmesi
        char[] tahmin = new char[secilenSehir.length()];
        for (int i = 0; i < secilenSehir.length(); i++) {
            tahmin[i] = '_';// seçilen kelimenin harf sayısı kadar bu işaretten koyuyoruz
        }

        // Fonksiyonu çağırıyoruz
        oyunuBaslat(secilenSehir, tahmin);
    }

    
    public static void oyunuBaslat(String kelime, char[] tahmin) {
        Scanner scanner = new Scanner(System.in);
        int puan = 100; // Başlangıç puanı

        long baslangicZamani = System.currentTimeMillis();//milisaniye olarak sayıyor

        while (true) {
            // Tahminin güncellenmiş haliyle kelimeyi göster
            kelimeyiGoster(tahmin);

            // Kullanıcıdan harf girişi istemek için
            System.out.print("Harf giriniz: ");
            char tahminHarfi = scanner.next().charAt(0);

            // Harfi kontrol et ve tahmin güncelle
            boolean dogruTahmin = harfiKontrolEt(kelime, tahmin, tahminHarfi);

            // Hata kontrolü ve puan güncelleme
            if (!dogruTahmin) {
                puan -= 5; // Yanlış tahmin için puan düşür
            }

            // Oyunu kazandı mı kontrolü
            if (kelimeTamamlandiMi(tahmin)) {
                long bitisZamani = System.currentTimeMillis();
                int sure = (int) ((bitisZamani - baslangicZamani) / 1000);

                // Puanlama sistemi
                int puanHesapla = puanlamaHesapla(sure);
                System.out.println("Tebrikler! Kelimeyi doğru tahmin ettiniz. Puanınız: " + puanHesapla);
                break;
            }
        }
        scanner.close();
    }

    // Kelimeyi gösteren metod
    public static void kelimeyiGoster(char[] tahmin) {
        for (char karakter : tahmin) {
            System.out.print(karakter + " ");
        }
        System.out.println();
    }

    // Harfi kontrol eden metod
    public static boolean harfiKontrolEt(String kelime, char[] tahmin, char harf) {
        boolean dogruTahmin = false;

        for (int i = 0; i < kelime.length(); i++) {
            if (kelime.charAt(i) == harf) {
                tahmin[i] = harf;
                dogruTahmin = true;
            }
        }

        if (!dogruTahmin) {
            System.out.println("Yanlış tahmin! -5 puan");
        }

        return dogruTahmin;
    }

    // Kelimenin tamamlandığını kontrol eden metod
    public static boolean kelimeTamamlandiMi(char[] tahmin) {
        for (char karakter : tahmin) {
            if (karakter == '_') {
                return false;
            }
        }
        return true;
    }

    // Puanlama hesaplayan metod
    public static int puanlamaHesapla(int sure) {
        if (sure <= 20) {
            return 100;//20 saniyeden az ise tam puan
        } else if (sure <= 30) {
            return 90;
        } else if (sure <= 40) {
            return 80;
        } else {
            return 70;
        }
    }
}
