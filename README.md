# Word_Guess_Game_React
A word guess game challenges players to identify a secret word using limited information or a restricted number of attempts. One person or a program provides clues, such as the word's length, category, or correct/incorrect letters after each guess.
#npm create vite@latest word-guess-game --template react
#cd word-guess-game
#npm install
#In your project folder navigate to src folder , next create components folder and create a file names "Game.jsx",add the below code
import { useState, useEffect } from "react";

const words = ["apple", "banana", "grapes", "orange", "mango"];

export default function Game() {
  const [selectedWord, setSelectedWord] = useState("");
  const [guessedLetters, setGuessedLetters] = useState([]);
  const [input, setInput] = useState("");
  const [message, setMessage] = useState("");

  useEffect(() => {
    const word = words[Math.floor(Math.random() * words.length)];
    setSelectedWord(word);
  }, []);

  const handleGuess = () => {
    if (!input) return;

    const letter = input.toLowerCase();
    if (guessedLetters.includes(letter)) {
      setMessage(`You already guessed "${letter}"`);
    } else if (!selectedWord.includes(letter)) {
      setMessage(`"${letter}" is not in the word`);
      setGuessedLetters([...guessedLetters, letter]);
    } else {
      setMessage(`Good! "${letter}" is correct`);
      setGuessedLetters([...guessedLetters, letter]);
    }
    setInput("");
  };

  const renderWord = () => {
    return selectedWord
      .split("")
      .map((letter) => (guessedLetters.includes(letter) ? letter : "_"))
      .join(" ");
  };

  const isWinner = selectedWord
    .split("")
    .every((letter) => guessedLetters.includes(letter));

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>Word Guess Game</h1>
      <p style={{ fontSize: "2rem", letterSpacing: "10px" }}>{renderWord()}</p>
      {isWinner && <p>Hooray! You guessed the word!</p>}
      {!isWinner && (
        <>
          <input
            type="text"
            maxLength="1"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            style={{ fontSize: "1.5rem", textAlign: "center" }}
          />
          <button
            onClick={handleGuess}
            style={{ marginLeft: "10px", fontSize: "1rem" }}
          >
            Guess
          </button>
        </>
      )}
      <p>{message}</p>
      <p>Guessed Letters: {guessedLetters.join(", ")}</p>
    </div>

#In "App.jsx" , paste the below code
import Game from "./components/Game";
function App() {
  return <Game />;
}

export default App;
  );
}

#In "index.html" , paste the below code

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Word Guess Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin-top: 50px;
    }
    #word {
      font-size: 2rem;
      letter-spacing: 10px;
      margin-bottom: 20px;
    }
    input {
      font-size: 1.5rem;
      text-align: center;
      width: 50px;
    }
    button {
      font-size: 1rem;
      margin-left: 10px;
      padding: 5px 10px;
    }
    #message {
      margin-top: 15px;
      font-weight: bold;
    }
    #guessed {
      margin-top: 10px;
    }
    #hint {
      margin-top: 10px;
      font-style: italic;
      color: #555;
    }
  </style>
</head>
<body>
  <h1>Word Guess Game</h1>
  <p id="category"></p>
  <p id="word"></p>
  <p id="hint"></p>

  <input type="text" id="letterInput" maxlength="1">
  <button onclick="guessLetter()">Guess</button>
  <button onclick="showHint()">Show Hint</button>
  <button onclick="resetGame()">Reset</button>

  <p id="message"></p>
  <p id="guessed">Guessed Letters: </p>

  <script>
    const wordCategories = {
      fruits: [
        { word: "apple", hint: "Keeps the doctor away" },
        { word: "banana", hint: "Yellow and curved" },
        { word: "grapes", hint: "Small, round, grows in bunches" },
        { word: "orange", hint: "Citrus fruit, also a color" },
        { word: "mango", hint: "King of fruits in India" },
        { word: "pineapple", hint: "Tropical fruit with spiky skin" },
        { word: "kiwi", hint: "Brown outside, green inside" },
        { word: "pear", hint: "Sweet fruit, teardrop shaped" },
        { word: "peach", hint: "Soft, fuzzy skin fruit" },
        { word: "plum", hint: "Small, purple or red fruit" }
      ],
      animals: [
        { word: "tiger", hint: "Striped big cat" },
        { word: "elephant", hint: "Largest land animal with a trunk" },
        { word: "giraffe", hint: "Tall animal with a long neck" },
        { word: "lion", hint: "King of the jungle" },
        { word: "monkey", hint: "Likes to swing on trees" },
        { word: "kangaroo", hint: "Marsupial that hops" },
        { word: "zebra", hint: "Black and white striped horse-like animal" },
        { word: "wolf", hint: "Lives in packs and howls" },
        { word: "fox", hint: "Cunning small wild animal" },
        { word: "bear", hint: "Large mammal, hibernates in winter" }
      ],
      colors: [
        { word: "red", hint: "Color of fire and roses" },
        { word: "blue", hint: "Color of sky and ocean" },
        { word: "green", hint: "Color of grass and leaves" },
        { word: "yellow", hint: "Color of the sun and bananas" },
        { word: "orange", hint: "A fruit and a color" },
        { word: "purple", hint: "Color of grapes and royalty" },
        { word: "pink", hint: "Light red color" },
        { word: "black", hint: "Color of night" },
        { word: "white", hint: "Color of snow" },
        { word: "gray", hint: "Between black and white" }
      ],
      countries: [
        { word: "india", hint: "Country in South Asia" },
        { word: "france", hint: "Famous for Eiffel Tower" },
        { word: "brazil", hint: "Known for Carnival and football" },
        { word: "japan", hint: "Land of the rising sun" },
        { word: "germany", hint: "Famous for Oktoberfest" },
        { word: "canada", hint: "Maple leaf country" },
        { word: "egypt", hint: "Land of pyramids" },
        { word: "china", hint: "Great Wall is here" },
        { word: "russia", hint: "Largest country by area" },
        { word: "mexico", hint: "Famous for tacos and tequila" }
      ],
      vehicles: [
        { word: "car", hint: "Four-wheeled vehicle" },
        { word: "bus", hint: "Carries many passengers" },
        { word: "bicycle", hint: "Pedal-powered two-wheeler" },
        { word: "airplane", hint: "Flies in the sky" },
        { word: "train", hint: "Runs on tracks" },
        { word: "motorcycle", hint: "Two-wheeled motor vehicle" },
        { word: "ship", hint: "Sails on water" },
        { word: "helicopter", hint: "Can hover in air" },
        { word: "submarine", hint: "Travels underwater" },
        { word: "scooter", hint: "Small motorized two-wheeler" }
      ]
    };

    let selectedCategory, selectedWordObj, guessedLetters = [];

    function pickRandomWord() {
      const categories = Object.keys(wordCategories);
      selectedCategory = categories[Math.floor(Math.random() * categories.length)];
      const words = wordCategories[selectedCategory];
      selectedWordObj = words[Math.floor(Math.random() * words.length)];
      guessedLetters = [];
      document.getElementById('category').textContent = `Category: ${selectedCategory}`;
      document.getElementById('message').textContent = '';
      document.getElementById('guessed').textContent = 'Guessed Letters: ';
      document.getElementById('hint').textContent = '';
      renderWord();
    }

    function renderWord() {
      const display = selectedWordObj.word
        .split('')
        .map(letter => guessedLetters.includes(letter) ? letter : '_')
        .join(' ');
      document.getElementById('word').textContent = display;
      if (!display.includes('_')) {
        document.getElementById('message').textContent = "Hooray! You guessed the word!";
      }
    }

    function guessLetter() {
      const input = document.getElementById('letterInput').value.toLowerCase();
      document.getElementById('letterInput').value = '';

      if (!input) return;

      if (guessedLetters.includes(input)) {
        document.getElementById('message').textContent = `You already guessed "${input}"`;
      } else if (!selectedWordObj.word.includes(input)) {
        document.getElementById('message').textContent = `"${input}" is not in the word`;
        guessedLetters.push(input);
      } else {
        document.getElementById('message').textContent = `Good! "${input}" is correct`;
        guessedLetters.push(input);
      }

      document.getElementById('guessed').textContent = `Guessed Letters: ${guessedLetters.join(', ')}`;
      renderWord();
    }

    function showHint() {
      document.getElementById('hint').textContent = `Hint: ${selectedWordObj.hint}`;
    }

    function resetGame() {
      pickRandomWord();
    }

    
    pickRandomWord();
  </script>
</body>
</html>

#Open terminal in the project, run the command "npm run dev" , it opens the development server in the port  "http://localhost:5173/"
