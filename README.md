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
 List<String> sebet = [];

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

 void sebeteElave(String ad) {
   setState(() {
     sebet.add('$ad: ${dermanlar[ad]}');
     netice = '$ad sebete elave edildi!';
   });
 }

 @override
 Widget build(BuildContext context) {
   String axtarilanAd = axtarController.text.trim().toLowerCase();

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
                         child: Text('Bagh'),
                       ),
                     ],
                   ),
                 );
               },
             ),
             if (sebet.isNotEmpty)
               Positioned(
                 right: 8,
                 top: 8,
                 child: Container(
                   padding: EdgeInsets.all(2),
                   decoration: BoxDecoration(
                     color: Colors.red,
                     borderRadius: BorderRadius.circular(10),
                   ),
                   child: Text(
                     '${sebet.length}',
                     style: TextStyle(color: Colors.white, fontSize: 12),
                   ),
                 ),
               ),
           ],
         ),
       ],
     ),
     body: Padding(
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
               child: Text(
                 'Axtar',
                 style: TextStyle(color: Colors.white, fontSize: 16),
               ),
             ),
           ),
           SizedBox(height: 16),
           if (netice.isNotEmpty)
             Container(
               width: double.infinity,
               padding: EdgeInsets.all(16),
               decoration: BoxDecoration(
                 color: Colors.white,
                 borderRadius: BorderRadius.circular(12),
                 border: Border.all(color: Color(0xFF1B8A5A)),
               ),
               child: Row(
                 mainAxisAlignment: MainAxisAlignment.spaceBetween,
                 children: [
                   Text(
                     netice,
                     style: TextStyle(
                       fontSize: 18,
                       color: Color(0xFF1B8A5A),
                       fontWeight: FontWeight.bold,
                     ),
                   ),
                   if (netice.contains('AZN'))
                     IconButton(
                       icon: Icon(Icons.add_shopping_cart,
                           color: Color(0xFF0D6EFD)),
                       onPressed: () => sebeteElave(axtarilanAd),
                     ),
                 ],
               ),
             ),
           SizedBox(height: 16),
           TextField(
             controller: adController,
             decoration: InputDecoration(
               hintText: 'Yeni derman adi...',
               prefixIcon: Icon(Icons.medication, color: Color(0xFF1B8A5A)),
               border: OutlineInputBorder(
                 borderRadius: BorderRadius.circular(12),
               ),
             ),
           ),
           SizedBox(height: 8),
           TextField(
             controller: qiymetController,
             decoration: InputDecoration(
               hintText: 'Qiymeti yaz...',
               prefixIcon: Icon(Icons.attach_money, color: Color(0xFF1B8A5A)),
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
                 backgroundColor: Color(0xFF0D6EFD),
                 padding: EdgeInsets.symmetric(vertical: 14),
                 shape: RoundedRectangleBorder(
                   borderRadius: BorderRadius.circular(12),
                 ),
               ),
               onPressed: elave,
               child: Text(
                 'Derman Elave Et',
                 style: TextStyle(color: Colors.white, fontSize: 16),
               ),
             ),
           ),
         ],
       ),
     ),
   );
 }
}
