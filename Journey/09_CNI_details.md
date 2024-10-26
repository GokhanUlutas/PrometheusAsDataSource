## CNI

CNI (Container Network Interface), Kubernetes ve diğer konteyner tabanlı platformlarda ağ bağlantısını yönetmek için kullanılan bir eklenti modelidir. **CNI**, konteyner ağ çözümleri geliştirmek için belirli kurallar ve yapı taşları sunarak konteynerlerin birbirleriyle ve dış dünya ile nasıl iletişim kuracağını standartlaştırır.

### CNI'nin Temel Amacı
Kubernetes gibi konteyner orkestrasyon platformlarında her bir pod'un:
- Kendi benzersiz IP adresine sahip olması,
- Kendi aralarında ve dış dünyayla iletişim kurabilmesi,
- Ağ trafiğinin izlenebilmesi ve güvenlik kurallarına uygun bir şekilde yönetilebilmesi gerekir.

CNI, bu gereksinimleri karşılamak için bir altyapı sunar ve ağ eklentilerinin Kubernetes gibi bir platforma kolayca entegre olmasını sağlar.

### CNI Nasıl Çalışır?
CNI, bir pod oluşturulurken:
1. **Ağ eklentisi** (CNI eklentisi), pod’a benzersiz bir ağ arabirimi ve IP adresi atar.
2. Eklenti, pod’un ağ kurulumunu ve gerektiğinde temizliğini sağlar.

### Kubernetes’te CNI Kullanımı
Kubernetes’te farklı ağ eklentilerini CNI ile kolayca entegre edebilirsiniz. En popüler CNI ağ eklentileri şunlardır:

- **Calico**: Ağ politikaları ve güvenlik özellikleri sunar.
- **Flannel**: Basit bir yapı sunar ve genellikle küçük veya orta ölçekli kümelerde kullanılır.
- **Weave**: Çok bölgeli ağ bağlantıları için uygundur.
- **Cilium**: Güvenlik için eBPF kullanarak yüksek performans sunar.

### Özet
CNI, Kubernetes içinde ağ yönetimi görevlerini yerine getiren bir eklenti modelidir. Bu sayede ağ yapılandırmalarını modüler, ölçeklenebilir ve yönetilebilir bir hale getirir, Kubernetes’in ağ gereksinimlerini standartlaştırılmış bir yapıda karşılamasını sağlar.