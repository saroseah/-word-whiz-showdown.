# -word-whiz-showdown.
Interactive ELA review game for 5th graders
import { useState } from 'react';
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { motion } from "framer-motion";

const vocabularyQuestions = [
  {
    points: 100,
    question: "Choose the correct word to complete the sentence: The kitten was so ___, it barely made a sound.",
    options: ["timid", "loud", "harsh", "frantic"],
    answer: "timid"
  },
  {
    points: 200,
    question: "What is a synonym for 'generous'?",
    options: ["stingy", "kind", "mean", "rude"],
    answer: "kind"
  }
];

const comprehensionQuestions = [
  {
    passage: "Lena tiptoed across the dewy grass, hoping not to wake the birds still sleeping in the trees. She clutched her notebook tightly, ready to sketch the sunrise.",
    questions: [
      {
        question: "Why did Lena walk quietly?",
        options: ["She was nervous", "She didn’t want to wake the birds", "She was sneaking away", "She didn’t want to get wet"],
        answer: "She didn’t want to wake the birds"
      }
    ]
  }
];

export default function WordWhizShowdown() {
  const [score, setScore] = useState([0, 0, 0, 0, 0]);
  const [currentQuestion, setCurrentQuestion] = useState(null);
  const [selectedPlayer, setSelectedPlayer] = useState(null);
  const [round, setRound] = useState("vocab");

  const handleAnswer = (option) => {
    if (currentQuestion.answer === option) {
      const newScores = [...score];
      newScores[selectedPlayer] += currentQuestion.points || 100;
      setScore(newScores);
    }
    setCurrentQuestion(null);
    setSelectedPlayer(null);
  };

  return (
    <div className="p-4 max-w-4xl mx-auto text-center">
      <h1 className="text-4xl font-bold mb-6">🎉 Word Whiz Showdown 🎉</h1>

      {currentQuestion ? (
        <Card className="mb-4">
          <CardContent className="p-4">
            <p className="text-xl font-semibold mb-2">{currentQuestion.question}</p>
            {currentQuestion.options.map((opt, i) => (
              <Button key={i} onClick={() => handleAnswer(opt)} className="m-2">
                {opt}
              </Button>
            ))}
          </CardContent>
        </Card>
      ) : (
        <div>
          <div className="grid grid-cols-5 gap-2 mb-6">
            {[0, 1, 2, 3, 4].map((player) => (
              <Card key={player} className="p-4">
                <p className="text-lg font-semibold">Player {player + 1}</p>
                <p className="text-2xl">{score[player]} pts</p>
                <Button
                  className="mt-2"
                  onClick={() => {
                    const q = vocabularyQuestions[Math.floor(Math.random() * vocabularyQuestions.length)];
                    setSelectedPlayer(player);
                    setCurrentQuestion(q);
                  }}
                >
                  Answer Question
                </Button>
              </Card>
            ))}
          </div>
          <Button onClick={() => setRound(round === "vocab" ? "comprehension" : "vocab")}>Switch to {round === "vocab" ? "Comprehension" : "Vocabulary"} Round</Button>
        </div>
      )}
    </div>
  );
}
