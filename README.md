# Codigo-fuente-app
class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();

  Future<void> _registerUser() async {
    try {
       final credential = await FirebaseAuth.instance.createUserWithEmailAndPassword(
         email: _emailController.text.trim(),
         password: _passwordController.text.trim(),
       );

       await FirebaseFirestore.instance.collection('usuarios').doc(credential.user!.uid).set({
         'email': credential.user!.email,
       'uid': credential.user!.uid,
         'registro': FieldValue.serverTimestamp(),
      });

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Registro exitoso')),
      );
      _navigateToHome();
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error: $e')),
      );
    }
  }

  Future<void> _loginUser() async {
    try {
      await FirebaseAuth.instance.signInWithEmailAndPassword(
        email: _emailController.text.trim(),
        password: _passwordController.text.trim(),
       );
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Inicio de sesión exitoso')),
      );
      _navigateToHome();
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error: $e')),
      );
    }
  }

  void _navigateToHome() {
    Navigator.pushReplacementNamed(context, '/home');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Inicio de Sesión'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              controller: _emailController,
              decoration: const InputDecoration(labelText: 'Correo electrónico'),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _passwordController,
              obscureText: true,
              decoration: const InputDecoration(labelText: 'Contraseña'),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _registerUser,
              child: const Text('Registrarse'),
            ),
            ElevatedButton(
              onPressed: _loginUser,
              child: const Text('Iniciar Sesión'),
            ),
          ],
        ),
      ),
    );
  }
}

import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';

void main() async {
  runApp(const MyApp());
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(
            seedColor: const Color.fromARGB(255, 16, 180, 1)), 
      ),
      home: const MyHomePage(title: 'Inicio'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.start,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            Center(
              child: Image.asset('assets/corazon.png', height: 100),
            ),
            const SizedBox(height: 300),
            const SizedBox(height: 50),

            // Botón de Autoevaluación-------------------------------------------------------------
            Center(
              child: ElevatedButton(
                onPressed: () {
                  // Navegar a la pantalla de Autoevaluación
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                        builder: (context) => const AutoevaluacionPage()),
                  );
                },
                style: ElevatedButton.styleFrom(
                  backgroundColor: const Color.fromARGB(255, 0, 253, 42),
                  padding:
                      const EdgeInsets.symmetric(vertical: 20, horizontal: 40),
                  minimumSize: const Size(double.infinity, 60),
                ),
                child: const Text(
                  'Autoevaluación',
                  style: TextStyle(
                      fontSize: 18, color: Color.fromARGB(255, 0, 0, 0)),
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Botón de Compartir experiencia------------------------------------------------------
            Center(
              child: ElevatedButton(
                onPressed: () {
                  // Navegar a la pantalla de Compartir experiencia
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                        builder: (context) => const CompartirExperienciaPage()),
                  );
                },
                style: ElevatedButton.styleFrom(
                  backgroundColor: const Color.fromARGB(255, 0, 253, 42),
                  padding:
                      const EdgeInsets.symmetric(vertical: 20, horizontal: 40),
                  minimumSize: const Size(double.infinity, 60),
                ),
                child: const Text(
                  'Compartir experiencia',
                  style: TextStyle(
                      fontSize: 18, color: Color.fromARGB(255, 0, 0, 0)),
                ),
              ),
            ),
            const SizedBox(height: 16),

            // Botón de Diario de pensamiento---------------------------------------------------------
            Center(
              child: ElevatedButton(
                onPressed: () {
                  // Navegar a la pantalla de Diario de pensamiento
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                        builder: (context) => const DiarioPensamientoPage()),
                  );
                },
                style: ElevatedButton.styleFrom(
                  backgroundColor: const Color.fromARGB(255, 0, 253, 42),
                  padding:
                      const EdgeInsets.symmetric(vertical: 20, horizontal: 20),
                  minimumSize: const Size(double.infinity, 60),
                ),
                child: const Text(
                  'Diario de pensamiento',
                  style: TextStyle(
                      fontSize: 18, color: Color.fromARGB(255, 0, 0, 0)),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

//para Autoevaluación-----------------------------------------------------------------
class AutoevaluacionPage extends StatefulWidget {
  const AutoevaluacionPage({super.key});

  @override
  State<AutoevaluacionPage> createState() => _AutoevaluacionPageState();
}

class _AutoevaluacionPageState extends State<AutoevaluacionPage> {
  final List<Map<String, dynamic>> _questions = [
    {
      'question': '¿Cómo te sientes?',
      'options': ['Bien', 'Regular', 'Mal'],
    },
    {
      'question': '¿Te has sentido productivo hoy?',
      'options': ['Sí', 'No', 'Algo'],
    },
    {
      'question': '¿Te sientes cansado mentalmente?',
      'options': ['Sí', 'No', 'A veces'],
    },
    {
      'question': '¿Te sientes estresado?',
      'options': ['Sí', 'No', 'A veces'],
    },
    {
      'question': '¿Me siento calmado/a y en paz con mis emociones?',
      'options': ['Sí', 'No', 'A veces'],
    },
  ];

  int _currentQuestionIndex = 0;
  String? _selectedAnswer;

  void _nextQuestion() {
    if (_currentQuestionIndex < _questions.length - 1) {
      setState(() {
        _currentQuestionIndex++;
        _selectedAnswer = null;
      });
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text("Has completado la autoevaluación")),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    final currentQuestion = _questions[_currentQuestionIndex];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Autoevaluación'),
        backgroundColor: const Color.fromARGB(255, 0, 243, 40),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Container(
              padding: const EdgeInsets.all(16),
              decoration: BoxDecoration(
                color: const Color.fromARGB(255, 235, 247, 223),
                borderRadius: BorderRadius.circular(12),
                boxShadow: [
                  BoxShadow(
                    color: Colors.grey.withOpacity(0.3),
                    spreadRadius: 2,
                    blurRadius: 5,
                    offset: const Offset(0, 3),
                  ),
                ],
              ),
              child: Text(
                currentQuestion['question'],
                style: const TextStyle(
                  fontSize: 24,
                  fontWeight: FontWeight.bold,
                  color: Colors.black,
                ),
                textAlign: TextAlign.center,
              ),
            ),
            const SizedBox(height: 30),

            // Opciones
            ...currentQuestion['options'].map<Widget>((option) {
              return Container(
                margin: const EdgeInsets.symmetric(vertical: 8),
                decoration: BoxDecoration(
                  border: Border.all(
                    color: _selectedAnswer == option
                        ? const Color.fromARGB(255, 0, 243, 40)
                        : Colors.grey,
                    width: 2,
                  ),
                  borderRadius: BorderRadius.circular(12),
                  color: _selectedAnswer == option
                      ? const Color.fromARGB(255, 208, 252, 220)
                      : Colors.white,
                  boxShadow: [
                    BoxShadow(
                      color: Colors.grey.withOpacity(0.3),
                      blurRadius: 5,
                      offset: const Offset(0, 3),
                    ),
                  ],
                ),
                child: ListTile(
                  leading: Icon(
                    _selectedAnswer == option
                        ? Icons.check_circle
                        : Icons.circle_outlined,
                    color: _selectedAnswer == option
                        ? const Color.fromARGB(255, 0, 243, 40)
                        : Colors.grey,
                  ),
                  title: Text(
                    option,
                    style: TextStyle(
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                      color: _selectedAnswer == option
                          ? const Color.fromARGB(255, 0, 243, 40)
                          : Colors.black,
                    ),
                  ),
                  onTap: () {
                    setState(() {
                      _selectedAnswer = option;
                    });
                  },
                ),
              );
            }).toList(),

            const SizedBox(height: 30),

            // Botón siguiente
            ElevatedButton(
              onPressed: _selectedAnswer == null ? null : _nextQuestion,
              style: ElevatedButton.styleFrom(
                backgroundColor: const Color.fromARGB(255, 0, 243, 40),
                padding:
                    const EdgeInsets.symmetric(vertical: 16, horizontal: 40),
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(12),
                ),
              ),
              child: const Text(
                'Siguiente pregunta',
                style: TextStyle(fontSize: 18, color: Colors.white),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

//para Compartir experiencia----------------------------------------------------------
class CompartirExperienciaPage extends StatefulWidget {
  const CompartirExperienciaPage({super.key});

  @override
  State<CompartirExperienciaPage> createState() =>
      _CompartirExperienciaPageState();
}

class _CompartirExperienciaPageState extends State<CompartirExperienciaPage> {
  final TextEditingController _controller = TextEditingController();

  void _compartirExperiencia() {
    final experiencia = _controller.text;

    if (experiencia.isNotEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Experiencia compartida: $experiencia')),
      );
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
            content: Text('Por favor, escribe algo para compartir.')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Compartir experiencia'),
        backgroundColor: const Color.fromARGB(255, 0, 243, 40),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Escribe como te sientes hoy',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            Container(
              padding: const EdgeInsets.all(8.0),
              decoration: BoxDecoration(
                color: Colors.white,
                borderRadius: BorderRadius.circular(12),
                boxShadow: [
                  BoxShadow(
                    color: Colors.grey.withOpacity(0.5),
                    spreadRadius: 2,
                    blurRadius: 5,
                    offset: const Offset(0, 3),
                  ),
                ],
              ),
              child: TextField(
                controller: _controller,
                maxLines: 20,
                decoration: const InputDecoration(
                  hintText: 'Escribe aquí recuerda que no estas solo...',
                  border: InputBorder.none,
                ),
                style: const TextStyle(fontSize: 16),
              ),
            ),
            const SizedBox(height: 20),
            Center(
              child: ElevatedButton(
                onPressed: _compartirExperiencia,
                style: ElevatedButton.styleFrom(
                  backgroundColor: const Color.fromARGB(255, 0, 243, 40),
                  padding:
                      const EdgeInsets.symmetric(vertical: 16, horizontal: 40),
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(8),
                  ),
                ),
                child: const Text(
                  'Compartir',
                  style: TextStyle(fontSize: 18, color: Colors.white),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

//Diario de Pensamiento-----------------------------------------------------------
class DiarioPensamientoPage extends StatefulWidget {
  const DiarioPensamientoPage({super.key});

  @override
  State<DiarioPensamientoPage> createState() => _DiarioPensamientoPageState();
}

class _DiarioPensamientoPageState extends State<DiarioPensamientoPage> {
  final TextEditingController _controller = TextEditingController();
  final List<String> _pensamientos = [
    'Primer pensamiento...',
    'Segundo pensamiento...',
    'Tercer pensamiento...',
  ];
  int _currentIndex = 0;

  void _prevPensamiento() {
    setState(() {
      if (_currentIndex > 0) {
        _currentIndex--;
        _controller.text = _pensamientos[_currentIndex];
      }
    });
  }

  void _nextPensamiento() {
    setState(() {
      if (_currentIndex < _pensamientos.length - 1) {
        _currentIndex++;
        _controller.text = _pensamientos[_currentIndex];
      }
    });
  }

  void _guardarPensamiento() {
    _pensamientos[_currentIndex] = _controller.text;
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Pensamiento guardado: "${_controller.text}"')),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Diario de Pensamiento'),
        backgroundColor: const Color.fromARGB(255, 0, 243, 40),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            const Text(
              'Escribe tu pensamiento del día:',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            Container(
              padding: const EdgeInsets.all(8.0),
              decoration: BoxDecoration(
                color: Colors.white,
                borderRadius: BorderRadius.circular(12),
                boxShadow: [
                  BoxShadow(
                    color: Colors.grey.withOpacity(0.5),
                    spreadRadius: 2,
                    blurRadius: 5,
                    offset: const Offset(0, 3),
                  ),
                ],
              ),
              child: TextField(
                controller: _controller,
                maxLines: 20,
                decoration: const InputDecoration(
                  hintText: 'Escribe aquí...',
                  border: InputBorder.none,
                ),
                style: const TextStyle(fontSize: 16),
              ),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                IconButton(
                  icon: const Icon(Icons.arrow_left),
                  onPressed: _prevPensamiento,
                ),
                ElevatedButton(
                  onPressed: _guardarPensamiento,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: const Color.fromARGB(255, 0, 243, 40),
                    padding: const EdgeInsets.symmetric(
                        vertical: 16, horizontal: 40),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(8),
                    ),
                  ),
                  child: const Text(
                    'Guardar Pensamiento',
                    style: TextStyle(fontSize: 16, color: Colors.white),
                  ),
                ),
                IconButton(
                  icon: const Icon(Icons.arrow_right),
                  onPressed: _nextPensamiento,
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
