<h1 align="center"> Cosmos Node Komutları (DURDURULDU) </h1>
<div align="center">

![Cosmos-ATOM-silent-has-tripled-in-recent-times-What](https://user-images.githubusercontent.com/76253089/221367235-ef56fc75-ad44-41d9-8e91-20704ad3806a.jpg)

</div>

### Cosmos SDK projelerinin hepsi aynı altyapı üzerine kurulmuştur ve node çalıştırırken yapacağımız işlemler için aynı komutları kullanırız fakat bu komutlarda bazı değişkenler vardır bunlar:

`nodeismi:` Tamamına yakınında projenin isminin sonuna d eklenmiş hali node ismidir. sei için seid, gitopia için gitopiad, stride için strided... <br>
`tokenismi:` Her ağın bir tokeni vardır ve bu tokenin ismi genelde ağın ismi ile aynıdır fakat komutlarda bir tokenin milyonda biri birimi ile kullanırız bunun için de token isminin başına u eklenir . sei için usei gitopia için utlore stride için ustride... <br>
`chain id:` Her zincirin bir chain id'si vardır, bunu proje ekibi kafasına göre belirliyor sanırım, nodunu kurduğunuz ağın chain id'sini bilirsiniz zaten <br>
`cüzdanismi:` Cüzdan oluştururken ismini siz belirlersiniz <br>
``moniker:` Bu da validatörünüzün ismidir ve kendiniz belirlersiniz <br>

## senkronizasyon durumu:
``` nodeismi status 2>&1 | jq .SyncInfo ```

## validatör durumu:
```nodeismi status 2>&1 | jq .ValidatorInfo```

## node durumu:​ <br>
```nodeismi status 2>&1 | jq .NodeInfo```
 
## cüzdan oluşturma​ <br>
bu komuttan sonra şifre oluşturmanızı ister (cüzdan şifresi) <br>
```nodeismi keys add cüzdanismi```

## var olan cüzdanı recover (geri getirmek) etmek :​ <br>
bu komuttan sonra mnemonic girmenizi ister <br>
```nodeismi keys add cüzdanismi -–recover```

## mevcut cüzdan(lar)ı listeleme :​ <br>
```nodeismi keys list```

## cüzdan silme:​ <br>
```nodeismi keys delete cüzdanismi```

## cüzdan bakiyesini öğrenme:​ <br>
```nodeismi query bank balances cüzdan adresi```

# Transaction komutlarından sonra cüzdan şifresi ister şifreyi girdikten sonra işlemi onaylıyor musunuz diye sorar y/n. y dedikten sonra bir txhash verir bunu explorerda arattığınızda SUCCESS (başarılı) veya FAİLED (başarısız) olduğunu görürsünüz. Başarısız olan işlemlerin hata mesajında sebebi yazar.

## bu komuta opsiyonel olarak --website --identity --details flaglarını da ekleyebilirsiniz. --amount kısmındaki miktarı cüzdanınızdaki bakiyeye göre belirlersiniz.

```
nodeismi tx staking create-validator \
--amount 000000tokenismi \
--from cüzdanismi \
--commission-max-change-rate "0.01" \
--commission-max-rate "0.2" \
--commission-rate "0.07" \
--min-self-delegation "1" \
--pubkey $(nodeismi tendermint show-validator) \
--moniker moniker \
--chain-id chain id \
--fees 250tokenismi
```

## örnek olarak gitopiada validatör oluşturmak için kullandığım komut:

```
gitopiad tx staking create-validator \
--amount 8000000utlore \
--from socrates \
--commission-max-change-rate "0.01" \
--commission-max-rate "0.2" \
--commission-rate "0.06" \
--min-self-delegation "1" \
--pubkey $(gitopiad tendermint show-validator) \
--moniker Socrates \
--website "https://twitter.com/0xSocrates_"
--identity 52B4347D67822C
--chain-id gitopia-janus-testnet-2
```

## delegate:​ <br>
valoper adresini explorerdan öğrenebilirsiniz <br>
```nodeismi tx staking delegate <valoperadresi> 0000000tokenismi --from cüzdanismi --chain-id chain id --fees 250tokenismi --gas auto```

örnek <br>
```gitopiad tx staking delegate gitopiavaloper1n55ayalxs0vmx2xqzgeav45x5qwa5qwezdlt7r 1000000utlore --from socrates --chain-id gitopia-janus-testnet-2 --fees 250utlore --gas auto```

## redelegate:​ <br>
1.valoper adresi tokenlerin stakede bulunduğu adres 2. valoper adresi ise redelegate etmek istediğiniz adres <br>
```nodeismi tx staking redelegate <1.valoperaddresi> <2.valoperadresi> 0000000tokenismi --from cüzdanismi --chain-id chain id --fees 250tokenismi --gas auto```

örnek gitopia için kullandığım komut: <br>
```gitopiad tx staking redelegate gitopiavaloper1n55ayalxs0vmx2xqzgeav45x5qwa5qwezdlt7r gitopiavaloper1vpxhyqh3qug8zyv7n89vwm3hvhfw7mxpe2dg28 1000000utlore --from socrates --chain-id gitopia-janus-testnet-2 --fees 250utlore --gas auto```

## başka cüzdana transfer:​ <br>
eğer komut hata verirse cüzdanismi yerine cüzdan adresini yazarak deneyin <br>
```nodeismi tx bank send cüzdanismi <hedefcüzdanadresi> 000000tokenismi --chain-id chain id --fees 250tokenismi --gas auto```

örnek: <br>
```gitopiad tx bank send socrates gitopia1u4tpzghqkumfa8dza6zl028jfxex6nkl7pj6ta 10utlore --chain-id gitopia-janus-testnet-2 --fees 250utlore --gas auto```

## kendi validatörünüze ait komisyon ve ödülleri çekme:​ <br>
```nodeismi tx distribution withdraw-rewards <valoperadresiniz> --commission --from cüzdanismi --chain-id chain id --fees 250tokenismi --gas auto```

## bütün validatörlerden ödülleri çekme(başka validatöre delege ettiyseniz):​ <br>
```nodeismi tx distribution withdraw-all-rewards --from cüzdanismi --chain-id chain id --fees 250tokenismi --gas auto```

## validatör unjail komutu (jailden kurtulma):​ <br>
```nodeismi tx slashing unjail --from cüzdanismi --chain-id chain id --gas auto```

## proposallarda oy kullanma:​ <br>
bu komuttaki 1 proposal numarasıdır kaçıncı proposala oy veriyorsanız onun numarsını yazarsınız. yes kısmı ise kullandığınız oy no yazabilirsiniz
```nodeismi tx gov vote 1 yes --from cüzdanismi --chain-id chain id --gas auto –y```
