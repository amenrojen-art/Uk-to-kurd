import 'package:flutter/material.dart';
import 'dart:html' as html;
import 'dart:math';

void main() => runApp(MaterialApp(home: MyStore(), debugShowCheckedModeBanner: false));

class MyStore extends StatefulWidget {
  @override
  _MyStoreState createState() => _MyStoreState();
}

class _MyStoreState extends State<MyStore> {
  final String myEmail = "rojenamin1@outlook.com";
  final List<Map<String, String>> products = [
    {"name": "سێتی ناوماڵ", "price": "15,000", "img": "https://i.ibb.co/r1PLh8N/image.jpg"},
    {"name": "شامپۆ کرێم ناوچاو", "price": "20,000", "img": "https://i.ibb.co/nMJ4CP2r/image.jpg"},
    {"name": "سیرۆمی گارنیەر", "price": "20,000", "img": "https://i.ibb.co/FbbspstH/image.jpg"},
    {"name": "جلوبەرگ", "price": "25,000", "img": "https://i.ibb.co/twwmLtnD/image.jpg"},
    {"name": "مەکیاجی ئافرەتان", "price": "30,000", "img": "https://i.ibb.co/BHys8z8d/image.jpg"},
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('UK TO KURD SHOP'), centerTitle: true, backgroundColor: Colors.white),
      body: GridView.builder(
        padding: EdgeInsets.all(10),
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 2, childAspectRatio: 0.75),
        itemCount: products.length,
        itemBuilder: (context, i) {
          return GestureDetector(
            onTap: () => Navigator.push(context, MaterialPageRoute(builder: (c) => ProductDetails(product: products[i], myEmail: myEmail))),
            child: Card(
              child: Column(children: [
                Expanded(child: Image.network(products[i]['img']!, fit: BoxFit.cover)),
                Text(products[i]['name']!, style: TextStyle(fontWeight: FontWeight.bold)),
                Text(products[i]['price']! + " IQD", style: TextStyle(color: Colors.green)),
              ]),
            ),
          );
        },
      ),
    );
  }
}

class ProductDetails extends StatefulWidget {
  final Map<String, String> product;
  final String myEmail;
  ProductDetails({required this.product, required this.myEmail});
  @override
  _ProductDetailsState createState() => _ProductDetailsState();
}

class _ProductDetailsState extends State<ProductDetails> {
  final nameController = TextEditingController();
  final phoneController = TextEditingController();
  String selectedCity = "زاخۆ";
  String deliveryFee = "3,000";

  void updateCity(String city) {
    setState(() {
      selectedCity = city;
      deliveryFee = (city == "زاخۆ") ? "3,000" : (city == "دهۆک" || city == "هەولێر" ? "5,000" : "7,000");
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(widget.product['name']!)),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(20),
        child: Column(children: [
          Image.network(widget.product['img']!, height: 200),
          Text("نرخ: ${widget.product['price']} IQD", style: TextStyle(fontSize: 20, color: Colors.green)),
          Text("گەیاندن: ٣ هەفتە / کرێ: $deliveryFee", style: TextStyle(color: Colors.red)),
          TextField(controller: nameController, decoration: InputDecoration(labelText: 'ناوی کڕیار')),
          TextField(controller: phoneController, decoration: InputDecoration(labelText: 'مۆبایل')),
          DropdownButton<String>(
            value: selectedCity,
            onChanged: (n) => updateCity(n!),
            items: ['زاخۆ', 'دهۆک', 'هەولێر', 'سلێمانی'].map((v) => DropdownMenuItem(value: v, child: Text(v))).toList(),
          ),
          ElevatedButton(
            onPressed: () {
              String body = Uri.encodeComponent("ناو: ${nameController.text}\nمۆبایل: ${phoneController.text}\nشار: $selectedCity\nبەرهەم: ${widget.product['name']}");
              html.window.open("mailto:${widget.myEmail}?subject=Order&body=$body", "_self");
            },
            child: Text("ناردنی داواکاری"),
          ),
        ]),
      ),
    );
  }
}

