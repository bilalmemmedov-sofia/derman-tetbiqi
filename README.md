import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    theme: ThemeData(
      colorScheme: ColorScheme.fromSeed(
        seedColor: Color(0xFF1B8A5A),
      ),
    ),
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
  String axtarilanAd = '';
  List<String> sebet = [];

  Map<String, String> dermanlar = {
    'paracetamol': '1.60 AZN',
    'nimesil': '13.43 AZN',
  };

  Map<String, int> endirimler = {
    'paracetamol': 20,
    'nimesil': 10,
  };

  void axtar() {
    String ad = axtarController.text.trim().toLowerCase();
    setState(() {
      axtarilanAd = ad;
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

  void sebeteElave(String ad) {
    setState(() {
      sebet.add('$ad: ${dermanlar[ad]}');
      netice = '$ad sebete elave edildi!';
    });
  }

  @override
  Widget build(BuildContext context) {
    bool endirimVar = endirimler.containsKey(axtarilanAd);
    double kohneQiymet = 0;
    double yeniQiymet = 0;

    if (endirimVar && dermanlar.containsKey(axtarilanAd)) {
      kohneQiymet = double.parse(
          dermanlar[axtarilanAd]!.replaceAll(' AZN', ''));
      yeniQiymet = kohneQiymet * (1 - endirimler[axtarilanAd]! / 100);
      print('Yeni qiymet: $yeniQiymet');
    }

    return Scaffold(
      backgroundColor: Color(0xFFF0F8F4),
      appBar: AppBar(
        backgroundColor: Color(0xFF1B8A5A),
        title: Text(
          'Derman Qiymatlari',
          style: TextStyle(
            color: Colors.white,
            fontWeight: FontWeight.bold,
          ),
        ),
        centerTitle: true,
        actions: [
          Stack(
            children: [
              IconButton(
                icon: Icon(Icons.shopping_cart, color: Colors.white),
                onPressed: () {
                  showDialog(
                    context: context,
                    builder: (context) => AlertDialog(
                      title: Text('Sebet'),
                      content: sebet.isEmpty
                          ? Text('Sebet boshdur!')
                          : Column(
                              mainAxisSize: MainAxisSize.min,
                              children: sebet
                                  .map((item) => Text(item))
                                  .toList(),
                            ),
                      actions: [
                        TextButton(
                          onPressed: () => Navigator.pop(context),
                          child: Text('Bağla'),
                        ),
                      ],
                    ),
                  );
                },
              ),
            ],
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              controller: axtarController,
              decoration: InputDecoration(
                hintText: 'Derman axtar...',
                prefixIcon: Icon(Icons.search, color: Color(0xFF1B8A5A)),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(12),
                ),
              ),
            ),
            SizedBox(height: 8),
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(
                  backgroundColor: Color(0xFF1B8A5A),
                  padding: EdgeInsets.symmetric(vertical: 14),
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(12),
                  ),
                ),
                onPressed: axtar,
                child: Text('Axtar', style: TextStyle(color: Colors.white)),
              ),
            ),
            Center(child: Text(netice)),
          ],
        ),
      ),
    );
  }
}