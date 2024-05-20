# takvim_yapragi

import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter/material.dart';

class Bilgi {
String id;
DateTime tarih;
String metin;
String kaynak;
int begeniSayisi;

Bilgi({
required this.id,
required this.tarih,
required this.metin,
required this.kaynak,
required this.begeniSayisi,
});

factory Bilgi.fromMap(Map<String, dynamic> data) {
return Bilgi(
id: data['id'] as String,
tarih: data['tarih'] as DateTime,
metin: data['metin'] as String,
kaynak: data['kaynak'] as String,
begeniSayisi: data['begeniSayisi'] as int,
);
}

Map<String, dynamic> toMap() {
return {
'id': id,
'tarih': tarih,
'metin': metin,
'kaynak': kaynak,
'begeniSayisi': begeniSayisi,
};
}
}

class Kullanici {
String id;
String kullaniciAdi;
String profilResmi;
List<String> takipEdilenler;
List<String> takipçiler;

Kullanici({
required this.id,
required this.kullaniciAdi,
required this.profilResmi,
required this.takipEdilenler,
required this.takipçiler,
});
}

void main() {
runApp(MyApp());
}

class MyApp extends StatelessWidget {
@override
Widget build(BuildContext context) {
return MaterialApp(
title: 'Takvim Yaprağı', // Uygulama Adı Değiştirildi
theme: ThemeData(
primarySwatch: Colors.blue,
),
home: MyHomePage(),
);
}
}

class MyHomePage extends StatefulWidget {
@override
_MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
List<Bilgi> bilgiler = [];
Kullanici kullanici = Kullanici();

@override
void initState() {
super.initState();

    // Firebase'e bağlan
    FirebaseFirestore.instance.collection('Bilgiler').get().then((querySnapshot) {
      for (var doc in querySnapshot.docs) {
        Bilgi bilgi = Bilgi.fromMap(doc.data());
        bilgiler.add(bilgi);
      }
      setState(() {});
    });
}

@override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(
backgroundColor: Colors.white, // Beyaz arkaplan
elevation: 0, // Gölgeyi kaldır
title: Text(
'Takvim Yaprağı', // Uygulama Adı Değiştirildi
style: TextStyle(
fontFamily: 'Roboto', // Yazı tipini "Roboto" olarak ayarla
fontWeight: FontWeight.bold, // Kalın yazı tipi
fontSize: 20, // Yazı tipi boyutunu 20 olarak ayarla
color: Colors.black, // Yazı tipi rengini siyah olarak ayarla
),
),
centerTitle: true, // Başlığı ortala
),
body: ListView.builder(
itemCount: bilgiler.length,
itemBuilder: (context, index) {
Bilgi bilgi = bilgiler[index];
return Card(
child: ListTile(
title: Text(bilgi.metin),
subtitle: Text('${bilgi.tarih.year}-${bilgi.tarih.month}-${bilgi.tarih.day}'),
),
);
},

