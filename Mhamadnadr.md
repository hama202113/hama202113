import 'package:flutter/material.dart';

void main() {
  runApp(const SoranSecurityApp());
}

class SoranSecurityApp extends StatelessWidget {
  const SoranSecurityApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'ئاسایشی سۆران',
      theme: ThemeData(
        primarySwatch: Colors.blueGrey,
        fontFamily: 'Rabar', // دەتوانیت فۆنتی کوردی لێبار بکەیت
      ),
      home: const ReportScreen(),
    );
  }
}

class ReportScreen extends StatefulWidget {
  const ReportScreen({super.key});

  @override
  State<ReportScreen> createState() => _ReportScreenState();
}

class _ReportScreenState extends State<ReportScreen> {
  final _formKey = GlobalKey<FormState>();
  String? _selectedIssue;
  
  final List<String> _issueTypes = [
    'هەڕەشەکردن و سەرانەسەندنی ئەلیکترۆنی',
    'بڵاوکردنەوە یان هەڕەشەی وێنە و ڤیدیۆی ڕووت',
    'تۆمارکردنی سکاڵای هاککردنی ئەژمار',
    'بابەتی تری پەیوەندیدار بە ئاسایشی گشتی',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('بەشی پاراستن و سکاڵا - ئاسایشی سۆران'),
        centerTitle: true,
        backgroundColor: const Color(0xFF1B2A47),
      ),
      body: Directionality(
        textDirection: TextDirection.rtl,
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(20.0),
          child: Form(
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                // کارتێک بۆ ڕوونکردنەوە و دڵنیاییدان
                Card(
                  color: Colors.red.shade50,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(12),
                    side: BorderSide(color: Colors.red.shade200),
                  ),
                  child: const Padding(
                    padding: EdgeInsets.all(16.0),
                    child: Row(
                      children: [
                        Icon(Icons.security, color: Colors.red, size: 40),
                        SizedBox(width: 12),
                        Expanded(
                          child: Text(
                            'زانیارییەکانت بە تەواوی نهێنین و ڕاستەوخۆ دەگەنە دەست تیمی تایبەتمەندی ئاسایش.',
                            style: TextStyle(fontSize: 14, fontWeight: FontWeight.bold, color: Colors.red),
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
                const SizedBox(height: 20),

                // زانیاری کەسی
                const Text('زانیاری کەسی (ئارەزوومەندانه):', style: TextStyle(fontWeight: FontWeight.bold)),
                const SizedBox(height: 10),
                TextFormField(
                  decoration: const InputDecoration(
                    labelText: 'ناوی سیانی',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.person),
                  ),
                ),
                const SizedBox(height: 15),
                TextFormField(
                  keyboardType: TextInputType.phone,
                  decoration: const InputDecoration(
                    labelText: 'ژمارەی مۆبایل (بۆ پەیوەندیکردنەوە)',
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.phone),
                  ),
                ),
                const SizedBox(height: 20),

                // هەڵبژاردنی جۆری کێشەکە
                const Text('جۆری سەرپێچی یان هەڕەشە:', style: TextStyle(fontWeight: FontWeight.bold)),
                const SizedBox(height: 10),
                DropdownButtonFormField<String>(
                  value: _selectedIssue,
                  hint: const Text('جۆری تاوانەکە هەڵبژێرە'),
                  items: _issueTypes.map((String value) {
                    return DropdownMenuItem<String>(
                      value: value,
                      child: Text(value),
                    );
                  }).toList(),
                  onChanged: (newValue) {
                    setState(() {
                      _selectedIssue = newValue;
                    });
                  },
                  decoration: const InputDecoration(
                    border: OutlineInputBorder(),
                    prefixIcon: Icon(Icons.warning_amber_rounded),
                  ),
                ),
                const SizedBox(height: 20),

                // ڕوونکردنەوەی کێشەکە
                const Text('ڕوونکردنەوەی تەواوی ڕووداوەکە:', style: TextStyle(fontWeight: FontWeight.bold)),
                const SizedBox(height: 10),
                TextFormField(
                  maxLines: 4,
                  decoration: const InputDecoration(
                    hintText: 'تکایە بە شێوەیەکی ڕوون باسی هەڕەشەکە یان کەسی تۆمەتبار بکە...',
                    border: OutlineInputBorder(),
                  ),
                ),
                const SizedBox(height: 20),

                // بەشی هاوپێچ کردنی وێنە/بەڵگە
                OutlinedButton.icon(
                  onPressed: () {
                    // بۆ زیادکردنی وێنە یان سکڕینشۆت
                  },
                  icon: const Icon(Icons.attach_file),
                  label: const Text('هاوپێچکردنی بەڵگە (وێنە، سکڕینشۆت)'),
                  style: OutlinedButton.styleFrom(
                    minimumSize: const Size(double.infinity, 50),
                  ),
                ),
                const SizedBox(height: 25),

                // بؤتنێک بۆ ناردن
                SizedBox(
                  width: double.infinity,
                  height: 50,
                  child: ElevatedButton(
                    onPressed: () {
                      if (_formKey.currentState!.validate()) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(content: Text('نێردرا! تیمی ئاسایش بە زووترین کات پێداچوونەوە دەکات.')),
                        );
                      }
                    },
                    style: ElevatedButton.styleFrom(
                      backgroundColor: const Color(0xFF1B2A47),
                    ),
                    child: const Text('ناردنی زانیارییەکان', style: TextStyle(fontSize: 16, color: Colors.white)),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
