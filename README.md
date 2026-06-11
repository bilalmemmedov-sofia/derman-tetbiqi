import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: MyApp(),
  ));
}

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  TextEditingController axtarController = TextEditingController();
  TextEditingController adController = TextEditingController();
  TextEditingController qiymetController = TextEditingController();
  String netice = '';

  Map<String, String> dermanlar = {
    'paracetamol': '1.60 AZN',
    'nimesil': '13.43 AZN',
  };

  void axtar() {
    String ad = axtarController.text.trim().toLowerCase();
    setState(() {
      if (dermanlar.containsKey(ad)) {
        netice = '$ad: ${dermanlar[ad]}';
      } else {
        netice = 'Derman tapilmadi!';
      }
    });
  }

  void elave() {
    String ad = adController.text.trim().toLowerCase();
    String qiymet = qiymetController.text.trim();
    setState(() {
      dermanlar[ad] = '$qiymet AZN';
      adController.clear();
      qiymetController.clear();
      netice = '$ad elave edildi!';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Derman Qiymatlari'),
      ),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              controller: axtarController,
              decoration: InputDecoration(
                hintText: 'Derman axtar...',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 8),
            ElevatedButton(
              onPressed: axtar,
              child: Text('Axtar'),
            ),
            SizedBox(height: 20),
            Text(netice, style: TextStyle(fontSize: 20)),
            SizedBox(height: 20),
            TextField(
              controller: adController,
              decoration: InputDecoration(
                hintText: 'Yeni derman adi...',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 8),
            TextField(
              controller: qiymetController,
              decoration: InputDecoration(
                hintText: 'Qiymeti yaz...',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 8),
            ElevatedButton(
              onPressed: elave,
              child: Text('Derman Elave Et'),
            ),
          ],
        ),
      ),
    );
  }
}
