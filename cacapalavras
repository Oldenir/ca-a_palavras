// Flutter code base for Word Search Game

import 'package:flutter/material.dart';
import 'dart:math';
import 'dart:async';

void main() => runApp(WordSearchApp());

class WordSearchApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Word Search Game',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: EnterCodeScreen(),
    );
  }
}

class EnterCodeScreen extends StatelessWidget {
  final TextEditingController _codeController = TextEditingController();
  final TextEditingController _nameController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Enter Code')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              controller: _nameController,
              decoration: InputDecoration(labelText: 'Your Name'),
            ),
            TextField(
              controller: _codeController,
              decoration: InputDecoration(labelText: 'Enter code'),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                String playerName = _nameController.text;
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => GameScreen(playerName: playerName)),
                );
              },
              child: Text('Join'),
            ),
            SizedBox(height: 10),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => CreateGameScreen()),
                );
              },
              child: Text('Criar Jogo'),
            ),
          ],
        ),
      ),
    );
  }
}

class GameScreen extends StatefulWidget {
  final String playerName;

  GameScreen({required this.playerName});

  @override
  _GameScreenState createState() => _GameScreenState();
}

class _GameScreenState extends State<GameScreen> {
  int wordsFound = 0;
  late Timer _timer;
  int _start = 0;
  bool showWords = true;
  List<List<String>> grid = [];
  List<Offset> selectedCells = [];
  List<List<bool>> highlightedCells = [];
  List<String> validWords = ['FLUTTER', 'DART', 'MOBILE'];

  @override
  void initState() {
    super.initState();
    generateGrid();
    startTimer();
  }

  void startTimer() {
    _timer = Timer.periodic(Duration(seconds: 1), (timer) {
      setState(() {
        _start++;
      });
    });
  }

  @override
  void dispose() {
    _timer.cancel();
    super.dispose();
  }

  void generateGrid() {
    grid = List.generate(10, (_) => List.generate(10, (_) => String.fromCharCode(Random().nextInt(26) + 65)));
    highlightedCells = List.generate(10, (_) => List.generate(10, (_) => false));
  }

  void onCellTap(int row, int col) {
    setState(() {
      selectedCells.add(Offset(row.toDouble(), col.toDouble()));
      if (selectedCells.length >= 2) {
        String selectedWord = getSelectedWord(selectedCells.first, selectedCells.last);
        if (validWords.contains(selectedWord)) {
          highlightPath(selectedCells.first, selectedCells.last);
          wordsFound++;
        }
        selectedCells.clear();
      }
    });
  }

  String getSelectedWord(Offset start, Offset end) {
    String word = '';
    int dr = (end.dx - start.dx).sign.toInt();
    int dc = (end.dy - start.dy).sign.toInt();
    int r = start.dx.toInt();
    int c = start.dy.toInt();
    while (r != end.dx.toInt() + dr || c != end.dy.toInt() + dc) {
      word += grid[r][c];
      r += dr;
      c += dc;
    }
    return word;
  }

  void highlightPath(Offset start, Offset end) {
    int dr = (end.dx - start.dx).sign.toInt();
    int dc = (end.dy - start.dy).sign.toInt();
    int r = start.dx.toInt();
    int c = start.dy.toInt();
    while (r != end.dx.toInt() + dr || c != end.dy.toInt() + dc) {
      highlightedCells[r][c] = true;
      r += dr;
      c += dc;
    }
  }

  void finishGame() {
    _timer.cancel();
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => FinalScoreScreen(
          playerName: widget.playerName,
          score: wordsFound,
          timeElapsed: _start,
        ),
      ),
    );
  }

  Widget buildGrid() {
    return Column(
      children: List.generate(10, (i) => Row(
        mainAxisAlignment: MainAxisAlignment.center,
        children: List.generate(10, (j) => GestureDetector(
          onTap: () => onCellTap(i, j),
          child: Container(
            margin: EdgeInsets.all(2),
            padding: EdgeInsets.all(8),
            decoration: BoxDecoration(
              color: highlightedCells[i][j] ? Colors.green : Colors.white,
              border: Border.all(color: Colors.black),
            ),
            child: Text(
              grid[i][j],
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
          ),
        )),
      )),
    );
  }

  Widget buildWordList() {
    return Column(
      children: validWords.map((word) => Text(word, style: TextStyle(fontSize: 16))).toList(),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bem-vindo, ${widget.playerName}'),
        actions: [
          IconButton(
            icon: Icon(showWords ? Icons.visibility_off : Icons.visibility),
            onPressed: () {
              setState(() {
                showWords = !showWords;
              });
            },
          )
        ],
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Boa sorte, ${widget.playerName}! Encontre as palavras escondidas.',
              style: TextStyle(fontSize: 18),
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 10),
            Text('Tempo: $_start segundos'),
            SizedBox(height: 10),
            if (showWords) buildWordList(),
            SizedBox(height: 10),
            buildGrid(),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: finishGame,
              child: Text('Finalizar Jogo'),
            ),
          ],
        ),
      ),
    );
  }
}

class FinalScoreScreen extends StatelessWidget {
  final String playerName;
  final int score;
  final int timeElapsed;

  FinalScoreScreen({required this.playerName, required this.score, required this.timeElapsed});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Resultado Final')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              '$playerName, você encontrou $score palavra(s)!',
              style: TextStyle(fontSize: 20),
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 10),
            Text('Tempo total: $timeElapsed segundos', style: TextStyle(fontSize: 16)),
          ],
        ),
      ),
    );
  }
}

// A tela CreateGameScreen, e todas as outras funcionalidades seguem como já implementadas anteriormente.
