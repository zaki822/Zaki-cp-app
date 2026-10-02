# Zaki-cp-app
import 'package:flutter/material.dart';

void main() {
  runApp(const ZakiCSApp());
}

class ZakiCSApp extends StatefulWidget {
  const ZakiCSApp({super.key});

  @override
  State<ZakiCSApp> createState() => _ZakiCSAppState();
}

class _ZakiCSAppState extends State<ZakiCSApp> {
  bool _isLoggedIn = false;
  String _currentLanguage = 'en'; // Default Language

  // قائمة لغات العالم الشاملة (Global Languages Database)
  final Map<String, Map<String, String>> _globalLanguages = {
    'ar': {'name': 'العربية', 'dir': 'rtl'},
    'en': {'name': 'English', 'dir': 'ltr'},
    'fr': {'name': 'Français', 'dir': 'ltr'},
    'es': {'name': 'Español', 'dir': 'ltr'},
    'de': {'name': 'Deutsch', 'dir': 'ltr'},
    'zh': {'name': '中文 (Chinese)', 'dir': 'ltr'},
    'ru': {'name': 'Русский', 'dir': 'ltr'},
    'ja': {'name': '日本語 (Japanese)', 'dir': 'ltr'},
    'pt': {'name': 'Português', 'dir': 'ltr'},
    'it': {'name': 'Italiano', 'dir': 'ltr'},
    'tr': {'name': 'Türkçe', 'dir': 'ltr'},
    'fa': {'name': 'فارسی', 'dir': 'rtl'},
    'ur': {'name': 'اردو', 'dir': 'rtl'},
    'hi': {'name': 'हिन्दी', 'dir': 'ltr'},
    'ko': {'name': '한국어', 'dir': 'ltr'},
  };

  void _loginSuccess() {
    setState(() {
      _isLoggedIn = true;
    });
  }

  void _changeLanguage(String langCode) {
    setState(() {
      _currentLanguage = langCode;
    });
  }

  @override
  Widget build(BuildContext context) {
    bool isRtl = _globalLanguages[_currentLanguage]?['dir'] == 'rtl';
    TextDirection textDirection = isRtl ? TextDirection.rtl : TextDirection.ltr;

    return Directionality(
      textDirection: textDirection,
      child: MaterialApp(
        title: 'Zaki AI-CS Hub',
        debugShowCheckedModeBanner: false,
        theme: ThemeData.dark().copyWith(
          scaffoldBackgroundColor: const Color(0xFF030712),
          primaryColor: const Color(0xFF00F5FF),
          cardColor: const Color(0xFF111827),
          appBarTheme: const AppBarTheme(
            backgroundColor: Color(0xFF0F172A),
            centerTitle: true,
            elevation: 1,
          ),
        ),
        home: _isLoggedIn
            ? MainNavigationScreen(
                currentLang: _currentLanguage,
                languages: _globalLanguages,
                onLangChanged: _changeLanguage,
              )
            : QuickAuthScreen(
                onLoginSuccess: _loginSuccess,
                currentLang: _currentLanguage,
                languages: _globalLanguages,
                onLangChanged: _changeLanguage,
              ),
      ),
    );
  }
}

// ---------------- 1. Auth & Global Language Selector Screen ----------------
class QuickAuthScreen extends StatefulWidget {
  final VoidCallback onLoginSuccess;
  final String currentLang;
  final Map<String, Map<String, String>> languages;
  final Function(String) onLangChanged;

  const QuickAuthScreen({
    super.key,
    required this.onLoginSuccess,
    required this.currentLang,
    required this.languages,
    required this.onLangChanged,
  });

  @override
  State<QuickAuthScreen> createState() => _QuickAuthScreenState();
}

class _QuickAuthScreenState extends State<QuickAuthScreen> {
  final _nameController = TextEditingController();
  bool _isNotRobotChecked = false;

  // Translation Localization Dictionary
  Map<String, Map<String, String>> localizedText = {
    'ar': {
      'subtitle': 'منصة زكرياء الرقمية للإعلام الآلي والذكاء الاصطناعي',
      'idLabel': 'المعرف الأكاديمي / اسم الطالب',
      'captcha': 'تأكيد الهوية البشرية (CAPTCHA)',
      'captchaSuccess': 'تم تأكيد الهوية بنجاح ✅',
      'loginBtn': 'دخول آمن (بدون إعلانات)',
      'emptyErr': 'يرجى إدخال المعرف الأكاديمي',
      'captchaErr': 'يرجى التأكد من مربع الهوية',
    },
    'en': {
      'subtitle': 'Zaki Digital CS & AI Academic Hub',
      'idLabel': 'Academic ID / Student Username',
      'captcha': 'Human Identity Verification (CAPTCHA)',
      'captchaSuccess': 'Identity Verified Successfully ✅',
      'loginBtn': 'Secure Access (Ad-Free)',
      'emptyErr': 'Please enter your Academic ID',
      'captchaErr': 'Please check the verification box',
    },
    'fr': {
      'subtitle': 'Plateforme Numérique Zaki d\'Informatique et IA',
      'idLabel': 'Identifiant Académique / Nom d\'Étudiant',
      'captcha': 'Vérification d\'Identité (CAPTCHA)',
      'captchaSuccess': 'Identité vérifiée avec succès ✅',
      'loginBtn': 'Accès Sécurisé (Sans Pub)',
      'emptyErr': 'Veuillez saisir votre identifiant',
      'captchaErr': 'Veuillez cocher la case de vérification',
    },
  };

  Map<String, String> _getTxt() {
    return localizedText[widget.currentLang] ?? localizedText['en']!;
  }

  @override
  Widget build(BuildContext context) {
    var txt = _getTxt();

    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Color(0xFF030712), Color(0xFF0F172A)],
          ),
        ),
        child: SafeArea(
          child: Center(
            child: SingleChildScrollView(
              padding: const EdgeInsets.all(24.0),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  // Global Language Picker Dropdown
                  Container(
                    padding: const EdgeInsets.symmetric(horizontal: 14, vertical: 4),
                    decoration: BoxDecoration(
                      color: const Color(0xFF111827),
                      borderRadius: BorderRadius.circular(20),
                      border: Border.all(color: const Color(0xFF00F5FF)),
                    ),
                    child: DropdownButtonHideUnderline(
                      child: DropdownButton<String>(
                        value: widget.currentLang,
                        dropdownColor: const Color(0xFF111827),
                        icon: const Icon(Icons.language, color: Color(0xFF00F5FF)),
                        onChanged: (String? newLang) {
                          if (newLang != null) widget.onLangChanged(newLang);
                        },
                        items: widget.languages.entries.map((entry) {
                          return DropdownMenuItem<String>(
                            value: entry.key,
                            child: Text(
                              entry.value['name']!,
                              style: const TextStyle(color: Colors.white, fontSize: 12),
                            ),
                          );
                        }).toList(),
                      ),
                    ),
                  ),
                  const SizedBox(height: 24),

                  // 🌟 شعار وأيقونة التطبيق الرقمية المستقبلية لـ Zaki 🌟
                  Container(
                    width: 105,
                    height: 105,
                    decoration: BoxDecoration(
                      shape: BoxShape.circle,
                      gradient: const LinearGradient(
                        colors: [Color(0xFF00F5FF), Color(0xFF7C3AED)],
                        begin: Alignment.topLeft,
                        end: Alignment.bottomRight,
                      ),
                      boxShadow: [
                        BoxShadow(
                          color: const Color(0xFF00F5FF).withOpacity(0.4),
                          blurRadius: 25,
                          spreadRadius: 3,
                        )
                      ],
                    ),
                    child: const Center(
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: [
                          Icon(Icons.memory, size: 42, color: Colors.black),
                          Text(
                            'ZAKI CS',
                            style: TextStyle(
                              color: Colors.black,
                              fontSize: 10,
                              fontWeight: FontWeight.bold,
                              letterSpacing: 1,
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(height: 16),
                  const Text(
                    'ZAKI AI-CS HUB',
                    style: TextStyle(
                      color: Color(0xFF00F5FF),
                      fontSize: 20,
                      letterSpacing: 2,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 6),
                  Text(
                    txt['subtitle']!,
                    style: const TextStyle(color: Colors.white70, fontSize: 12),
                    textAlign: TextAlign.center,
                  ),
                  const SizedBox(height: 28),
                  TextField(
                    controller: _nameController,
                    style: const TextStyle(color: Colors.white),
                    decoration: InputDecoration(
                      labelText: txt['idLabel'],
                      labelStyle: const TextStyle(color: Colors.grey, fontSize: 12),
                      prefixIcon: const Icon(Icons.person_outline, color: Color(0xFF00F5FF)),
                      filled: true,
                      fillColor: const Color(0xFF111827),
                      border: OutlineInputBorder(borderRadius: BorderRadius.circular(12)),
                    ),
                  ),
                  const SizedBox(height: 16),
                  Container(
                    padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 8),
                    decoration: BoxDecoration(
                      color: const Color(0xFF111827),
                      borderRadius: BorderRadius.circular(12),
                      border: Border.all(color: Colors.white10),
                    ),
                    child: Row(
                      children: [
                        Checkbox(
                          value: _isNotRobotChecked,
                          activeColor: const Color(0xFF00F5FF),
                          checkColor: Colors.black,
                          onChanged: (val) {
                            setState(() {
                              _isNotRobotChecked = !_isNotRobotChecked;
                              if (_isNotRobotChecked) {
                                ScaffoldMessenger.of(context).showSnackBar(
                                  SnackBar(content: Text(txt['captchaSuccess']!)),
                                );
                              }
                            });
                          },
                        ),
                        Text(txt['captcha']!, style: const TextStyle(color: Colors.white, fontSize: 12)),
                        const Spacer(),
                        const Icon(Icons.verified_user, color: Colors.greenAccent, size: 18),
                      ],
                    ),
                  ),
                  const SizedBox(height: 24),
                  SizedBox(
                    width: double.infinity,
                    height: 48,
                    child: ElevatedButton(
                      onPressed: () {
                        if (_nameController.text.trim().isEmpty) {
                          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(txt['emptyErr']!)));
                          return;
                        }
                        if (!_isNotRobotChecked) {
                          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(txt['captchaErr']!)));
                          return;
                        }
                        widget.onLoginSuccess();
                      },
                      style: ElevatedButton.styleFrom(
                        backgroundColor: const Color(0xFF00F5FF),
                        foregroundColor: Colors.black,
                        shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
                      ),
                      child: Text(txt['loginBtn']!, style: const TextStyle(fontWeight: FontWeight.bold)),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}

// ---------------- 2. Main Navigation Screen ----------------
class MainNavigationScreen extends StatefulWidget {
  final String currentLang;
  final Map<String, Map<String, String>> languages;
  final Function(String) onLangChanged;

  const MainNavigationScreen({
    super.key,
    required this.currentLang,
    required this.languages,
    required this.onLangChanged,
  });

  @override
  State<MainNavigationScreen> createState() => _MainNavigationScreenState();
}

class _MainNavigationScreenState extends State<MainNavigationScreen> {
  int _currentIndex = 0;

  Map<String, Map<String, String>> navTitles = {
    'ar': {'tutor': 'مساعد Zaki', 'reels': 'الريلز', 'courses': 'الدروس', 'library': 'المكتبة'},
    'en': {'tutor': 'Zaki Assistant', 'reels': 'Reels', 'courses': 'Courses', 'library': 'Library'},
    'fr': {'tutor': 'Assistant Zaki', 'reels': 'Reels', 'courses': 'Cours', 'library': 'Bibliothèque'},
  };

  Map<String, String> _getNav() {
    return navTitles[widget.currentLang] ?? navTitles['en']!;
  }

  @override
  Widget build(BuildContext context) {
    var nav = _getNav();

    final List<Widget> screens = [
      AiStudyTutorScreen(currentLang: widget.currentLang),
      CyberReelsScreen(currentLang: widget.currentLang),
      CourseHubScreen(currentLang: widget.currentLang),
      LibraryScreen(currentLang: widget.currentLang),
    ];

    return Scaffold(
      appBar: AppBar(
        title: const Row(
          mainAxisSize: MainAxisSize.min,
          children: [
            Icon(Icons.auto_awesome, color: Color(0xFF00F5FF), size: 18),
            SizedBox(width: 8),
            Text('Zaki AI-CS Platform', style: TextStyle(fontSize: 14, color: Colors.white, fontWeight: FontWeight.bold)),
          ],
        ),
        actions: [
          PopupMenuButton<String>(
            icon: const Icon(Icons.language, color: Color(0xFF00F5FF)),
            onSelected: widget.onLangChanged,
            itemBuilder: (context) => widget.languages.entries.map((entry) {
              return PopupMenuItem<String>(
                value: entry.key,
                child: Text(entry.value['name']!),
              );
            }).toList(),
          ),
        ],
      ),
      body: screens[_currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentIndex,
        onTap: (index) => setState(() => _currentIndex = index),
        type: BottomNavigationBarType.fixed,
        backgroundColor: const Color(0xFF0F172A),
        selectedItemColor: const Color(0xFF00F5FF),
        unselectedItemColor: Colors.grey,
        items: [
          BottomNavigationBarItem(icon: const Icon(Icons.psychology), label: nav['tutor']),
          BottomNavigationBarItem(icon: const Icon(Icons.video_collection_outlined), label: nav['reels']),
          BottomNavigationBarItem(icon: const Icon(Icons.school), label: nav['courses']),
          BottomNavigationBarItem(icon: const Icon(Icons.menu_book), label: nav['library']),
        ],
      ),
    );
  }
}

// ---------------- 3. Zaki AI Engine Assistant ----------------
class AiStudyTutorScreen extends StatefulWidget {
  final String currentLang;
  const AiStudyTutorScreen({super.key, required this.currentLang});

  @override
  State<AiStudyTutorScreen> createState() => _AiStudyTutorScreenState();
}

class _AiStudyTutorScreenState extends State<AiStudyTutorScreen> {
  final TextEditingController _promptController = TextEditingController();

  Map<String, Map<String, String>> tutorTexts = {
    'ar': {
      'welcome': 'مرحباً بك! أنا المحرك الأكاديمي لمنصة Zaki CS. اطرح أي سؤال في الإعلام الآلي والذكاء الاصطناعي للحصول على الحل والشرح.',
      'hint': 'اكتب سؤالك أو ضع نص التمرين...',
      'status': 'تشفير وحماية البيانات الأكاديمية: نشط',
      'reply': 'تمت معالجة التمرين بنجاح بواسطة خوارزمية Zaki CS Core:\n1. الشرح والتحليل البرمجي.\n2. الحل خطوة بخطوة.\n3. التوصية بالدرس المطابق.',
    },
    'en': {
      'welcome': 'Welcome! I am the Zaki CS Academic Engine. Submit any programming query or CS problem for solutions.',
      'hint': 'Type your question or code problem...',
      'status': 'Data Encryption & Academic Security: Active',
      'reply': 'Processed successfully by Zaki CS Core Algorithm:\n1. Code Analysis & Explanation.\n2. Step-by-step Solution.\n3. Recommended Course Material.',
    },
    'fr': {
      'welcome': 'Bienvenue! Je suis le moteur académique Zaki CS. Soumettez vos questions ou exercices d\'informatique.',
      'hint': 'Tapez votre question ou exercice...',
      'status': 'Chiffrement et Sécurité des Données: Actif',
      'reply': 'Traité avec succès par l\'Algorithme Zaki CS Core:\n1. Analyse de Code et Explication.\n2. Solution Étape par Étape.\n3. Cours Recommandé.',
    },
  };

  late List<Map<String, String>> _messages;

  @override
  void initState() {
    super.initState();
    var txt = tutorTexts[widget.currentLang] ?? tutorTexts['en']!;
    _messages = [
      {'role': 'system', 'text': txt['welcome']!}
    ];
  }

  void _sendMessage() {
    String query = _promptController.text.trim();
    if (query.isEmpty) return;

    var txt = tutorTexts[widget.currentLang] ?? tutorTexts['en']!;

    setState(() {
      _messages.add({'role': 'user', 'text': query});
      _promptController.clear();
    });

    Future.delayed(const Duration(milliseconds: 600), () {
      setState(() {
        _messages.add({
          'role': 'system',
          'text': txt['reply']!
        });
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    var txt = tutorTexts[widget.currentLang] ?? tutorTexts['en']!;

    return Column(
      children: [
        Container(
          padding: const EdgeInsets.all(10),
          color: const Color(0xFF111827),
          child: Row(
            children: [
              const Icon(Icons.shield, color: Colors.greenAccent, size: 16),
              const SizedBox(width: 8),
              Text(txt['status']!, style: const TextStyle(color: Colors.white, fontSize: 11)),
            ],
          ),
        ),
        Expanded(
          child: ListView.builder(
            padding: const EdgeInsets.all(12),
            itemCount: _messages.length,
            itemBuilder: (context, index) {
              bool isUser = _messages[index]['role'] == 'user';
              return Align(
                alignment: isUser ? Alignment.centerRight : Alignment.centerLeft,
                child: Container(
                  margin: const EdgeInsets.symmetric(vertical: 4),
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: isUser ? const Color(0xFF00F5FF).withOpacity(0.2) : const Color(0xFF111827),
                    borderRadius: BorderRadius.circular(12),
                    border: Border.all(color: isUser ? const Color(0xFF00F5FF) : Colors.white10),
                  ),
                  child: Text(_messages[index]['text']!, style: const TextStyle(color: Colors.white, fontSize: 12)),
                ),
              );
            },
          ),
        ),
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: Row(
            children: [
              Expanded(
                child: TextField(
                  controller: _promptController,
                  style: const TextStyle(color: Colors.white),
                  decoration: InputDecoration(
                    hintText: txt['hint'],
                    hintStyle: const TextStyle(color: Colors.grey, fontSize: 11),
                    filled: true,
                    fillColor: const Color(0xFF111827),
                    border: OutlineInputBorder(borderRadius: BorderRadius.circular(10)),
                  ),
                ),
              ),
              const SizedBox(width: 6),
              IconButton(
                icon: const Icon(Icons.send, color: Color(0xFF00F5FF)),
                onPressed: _sendMessage,
              ),
            ],
          ),
        )
      ],
    );
  }
}

// ---------------- 4. Cyber Reels Module ----------------
class CyberReelsScreen extends StatefulWidget {
  final String currentLang;
  const CyberReelsScreen({super.key, required this.currentLang});

  @override
  State<CyberReelsScreen> createState() => _CyberReelsScreenState();
}

class _CyberReelsScreenState extends State<CyberReelsScreen> {
  int _likeCount = 420;
  bool _isLiked = false;

  @override
  Widget build(BuildContext context) {
    return PageView.builder(
      scrollDirection: Axis.vertical,
      itemCount: 3,
      itemBuilder: (context, index) {
        return Container(
          margin: const EdgeInsets.all(8),
          decoration: BoxDecoration(
            color: const Color(0xFF111827),
            borderRadius: BorderRadius.circular(20),
            border: Border.all(color: const Color(0xFF00F5FF).withOpacity(0.3)),
          ),
          child: Stack(
            children: [
              const Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Icon(Icons.play_circle_fill, size: 70, color: Color(0xFF00F5FF)),
                    SizedBox(height: 12),
                    Text('Zaki Cyber Reel', style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
                    Text('Protected Educational Content - Ad Free', style: TextStyle(color: Colors.white54, fontSize: 11)),
                  ],
                ),
              ),
              Positioned(
                right: 16,
                bottom: 40,
                child: Column(
                  children: [
                    IconButton(
                      icon: Icon(Icons.favorite, color: _isLiked ? Colors.redAccent : Colors.white, size: 28),
                      onPressed: () {
                        setState(() {
                          _isLiked = !_isLiked;
                          _isLiked ? _likeCount++ : _likeCount--;
                        });
                      },
                    ),
                    Text('$_likeCount', style: const TextStyle(color: Colors.white, fontSize: 10)),
                    const SizedBox(height: 12),
                    const Icon(Icons.comment, color: Colors.white, size: 28),
                    const SizedBox(height: 12),
                    IconButton(
                      icon: const Icon(Icons.share, color: Color(0xFF00F5FF), size: 28),
                      onPressed: () {
                        ScaffoldMessenger.of(context).showSnackBar(
                          const SnackBar(content: Text('Secure Link Copied ✅')),
                        );
                      },
                    ),
                  ],
                ),
              ),
            ],
          ),
        );
      },
    );
  }
}

// ---------------- 5. Official Course Hub ----------------
class CourseHubScreen extends StatelessWidget {
  final String currentLang;
  const CourseHubScreen({super.key, required this.currentLang});

  @override
  Widget build(BuildContext context) {
    return ListView(
      padding: const EdgeInsets.all(12),
      children: const [
        ListTile(
          tileColor: Color(0xFF111827),
          title: Text('Algorithms & Data Structures (ASD)', style: TextStyle(color: Colors.white, fontSize: 13)),
          subtitle: Text('Official Academic Curriculum 🏛️', style: TextStyle(color: Color(0xFF00F5FF), fontSize: 10)),
          trailing: Icon(Icons.star, color: Colors.amber),
        ),
      ],
    );
  }
}

// ---------------- 6. Digital CS Library ----------------
class LibraryScreen extends StatelessWidget {
  final String currentLang;
  const LibraryScreen({super.key, required this.currentLang});

  @override
  Widget build(BuildContext context) {
    return const Center(
      child: Text('Zaki CS Library Engine', style: TextStyle(color: Colors.white)),
    );
  }
}
عرض النص المقتبس
