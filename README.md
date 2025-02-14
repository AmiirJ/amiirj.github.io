# amiirj.github.io
<!DOCTYPE html>
<html lang="fi">
<head>
  <meta charset="UTF-8" />
  <title>Amiirin guss Kansainvälisen verotuksen kysymyspeli</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 2em;
      background: #f0f0f0;
    }
    h1 {
      color: #333;
    }
    .quiz-container {
      background: #fff;
      padding: 1em 2em;
      border-radius: 5px;
      max-width: 700px;
    }
    .question {
      margin: 1em 0;
      font-weight: bold;
    }
    .answers input {
      margin-right: 8px;
    }
    .feedback {
      margin-top: 1em;
      font-weight: bold;
    }
    button {
      margin-top: 1em;
      padding: 0.5em 1em;
      border: none;
      cursor: pointer;
      background-color: #007bff;
      color: white;
      border-radius: 4px;
    }
    button:hover {
      background-color: #0056b3;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <h1>Kansainvälisen verotuksen kysymyspeli</h1>
    <p>Kuunnelkaa broidit täs kaikist tenteist otettu kysymykset, jotka toistuu kerran tai useammi yht. 30 kysymyst. Jokases yks oikee vastaus ja peli syöttää ne satunnaisjärjestykses. Aloita painamalla "Aloita peli".</p>
    
    <button id="startBtn">Aloita peli</button>
    
    <div id="quiz" class="hidden">
      <div id="question" class="question"></div>
      <div id="answers" class="answers"></div>
      <button id="checkBtn">Tarkista vastaus</button>
      <div id="feedback" class="feedback"></div>
      <button id="nextBtn" class="hidden">Seuraava kysymys</button>
    </div>
    
    <div id="summary" class="hidden">
      <h2>Peli päättyi!</h2>
      <p id="score"></p>
    </div>
  </div>

<script>
  // Tässä on 30 kysymystä lyhennetyssä muodossa.
  // correct-avaimen arvo on oikean vaihtoehdon indeksi (0, 1, 2...).
  const questions = [
    {
      question: "1. Ristiriitatilanteessa etusija annetaan:",
      answers: [
        "a) EU-oikeuden normille",
        "b) Kansallisen lain normille",
        "c) Verosopimukselle"
      ],
      correct: 0
    },
    {
      question: "2. Verosopimusoikeuden kultainen sääntö tarkoittaa, että:",
      answers: [
        "a) Verosopimus laajentaa kansallista verotusta",
        "b) Verosopimus vain rajoittaa kansallista verotusta",
        "c) Verosopimus ei vaadi eduskuntakäsittelyä"
      ],
      correct: 1
    },
    {
      question: "3. OECD:n BEPS-projekti tarkoittaa:",
      answers: [
        "a) Business Entities Profit Shifting",
        "b) Base Erosion and Profit Shifting",
        "c) Base Erosion and Profit Sheltering"
      ],
      correct: 1
    },
    {
      question: "4. Suomessa kaksinkertaisen verotuksen poistamiseen käytetään ensisijaisesti:",
      answers: [
        "a) Vapautusmenetelmää (exemption)",
        "b) Hyvitysmenetelmää (credit)",
        "c) Vähennysmenetelmää (deduction)"
      ],
      correct: 1
    },
    {
      question: "5. Verosopimuksen mukaisen asuinvaltion ensisijainen kriteeri on:",
      answers: [
        "a) Kansalaisuus",
        "b) Elinetujen keskus",
        "c) Vakituinen koti"
      ],
      correct: 2
    },
    {
      question: "6. Verosopimusten mekaanikkosäännön soveltamiseen ei vaikuta:",
      answers: [
        "a) Työskentelyn kesto",
        "b) Työnantajan sijaintivaltio",
        "c) Onko työnantajalla kiinteä toimipaikka työskentelyvaltiossa"
      ],
      correct: 2
    },
    {
      question: "7. Ei ole kansainvälistä taloudellista kahdenkert. verotusta, jos:",
      answers: [
        "a) Sekä Suomi että toinen valtio verottavat Suomessa asuvaa henkilöä palkasta",
        "b) Suomi epää tytäryhtiön kauppahinnan vähennyksen, ulk. valtio verottaa koko kauppahinnan",
        "c) Suomi verottaa yhtiön voiton ja toinen valtio verottaa osingon saajaa"
      ],
      correct: 0
    },
    {
      question: "8. TVL 11 §:n tarkoittama olennainen side on:",
      answers: [
        "a) Suomesta saatava eläke",
        "b) Suomessa asuva aviopuoliso",
        "c) Kesäloman viettäminen Suomessa"
      ],
      correct: 1
    },
    {
      question: "9. Kuuden kuukauden verovapaussäännön soveltaminen edellyttää:",
      answers: [
        "a) Työskentelyä ulkomailla väh. 6 kk",
        "b) Oleskelua ulkomailla väh. 6 kk",
        "c) Työskentelyä 6 kk kalenterivuonna"
      ],
      correct: 0
    },
    {
      question: "10. 6 kk ulkomaantyöskentelysääntö ei sovellu, jos:",
      answers: [
        "a) Työskentely kestää tasan 6 kk",
        "b) Verosopimuksen mekaanikkosääntö soveltuu",
        "c) Työnantaja on ulkomainen"
      ],
      correct: 1
    },
    {
      question: "11. Palkka on aina Suomesta saatua, jos:",
      answers: [
        "a) Työnantaja on suomalainen yhtiö",
        "b) Työnantaja on Suomen valtio",
        "c) Työ tehdään pääasiassa Suomessa"
      ],
      correct: 1
    },
    {
      question: "12. Rajoitetusti verovelvollisen Suomesta saamia eläkkeitä verotetaan:",
      answers: [
        "a) Aina lähdeverona 35 %:lla",
        "b) Verotusmenettelylain mukaan progressiivisesti (jos Suomi saa verottaa)",
        "c) Ei koskaan veroteta Suomessa verosopimustilanteissa"
      ],
      correct: 1
    },
    {
      question: "13. Toimintoanalyysissä (siirtohinnoittelussa) käydään läpi:",
      answers: [
        "a) Toiminnot, riskit ja resurssit",
        "b) Mahd. vain liikevaihdon kehitys",
        "c) Osakkaiden yksityistalouden kulut"
      ],
      correct: 0
    },
    {
      question: "14. Etuyhteysyritysten sisäisten transaktioiden siirtohinnoittelu on:",
      answers: [
        "a) Kiellettyä",
        "b) Sallittua",
        "c) Välttämätöntä"
      ],
      correct: 1
    },
    {
      question: "15. Markkinaehtoperiaate edellyttää, että:",
      answers: [
        "a) Konserniyritykset eivät harjoita siirtohinnoittelua lainkaan",
        "b) Konserniyhtiöiden hinnoittelu oikaistaan riippumattoman osapuolen tasoon",
        "c) Sisäinen hintavertailu on vapaaehtoista"
      ],
      correct: 1
    },
    {
      question: "16. OECD:n malliverosopimuksen 9 artikla (verosopimus) käytännössä:",
      answers: [
        "a) Sallii siirtohinnoitteluoikaisut",
        "b) Kieltää siirtohinnoitteluoikaisut",
        "c) Edellyttää EU-tuomioistuimen hyväksyntää"
      ],
      correct: 0
    },
    {
      question: "17. Jos tytäryhtiö maksaa emolle korkoa yli markkinaehtoisen määrän, ylimenevä osa on:",
      answers: [
        "a) Korkoa",
        "b) Osinkoa",
        "c) Myyntivoittoa"
      ],
      correct: 1
    },
    {
      question: "18. Ulkomaisella yhtiöllä on Suomessa kiinteä toimipaikka, jos:",
      answers: [
        "a) Sillä on tytäryhtiö Suomessa",
        "b) Sillä on itsenäinen agentti Suomessa",
        "c) Sillä on epäitsenäinen agentti Suomessa, joka solmii sopimuksia yhtiön nimissä"
      ],
      correct: 2
    },
    {
      question: "19. Jos ulkom. yhtiön tosiasiallinen johtopaikka on Suomessa, yhtiö on:",
      answers: [
        "a) Rajoitetusti verovelvollinen Suomessa",
        "b) Yleisesti verovelvollinen Suomessa"
      ],
      correct: 1
    },
    {
      question: "20. Väliyhteisölaki voi soveltua, jos:",
      answers: [
        "a) Suom. yhtiö omistaa yli 50 % ulkomaisesta yhtiöstä, jonka verotus on selvästi kevyempi",
        "b) Suomessa asuva omistaa alle 10 % ulkomaisesta yhtiöstä",
        "c) Asuinvaltiossa maksettu vero on suurempi kuin Suomen yhtiövero"
      ],
      correct: 0
    },
    {
      question: "21. Väliyhteisölaki ei voi soveltua, jos:",
      answers: [
        "a) Ulkomainen yhteisö sijaitsee EU-valtiossa riippumatta olosuhteista",
        "b) Yhtiön tulot muodostuvat pääasiassa myynti- ja markkinointitoiminnasta",
        "c) Ulkomaisen yhteisön maksamat verot vastaavat Suomen tasoa"
      ],
      correct: 2
    },
    {
      question: "22. Ulkomaanveron hyvityksen enimmäismäärä Suomessa lasketaan:",
      answers: [
        "a) Maakohtaisesti, tulolähteittäin ja tulolajeittain",
        "b) Vain maakohtaisesti",
        "c) Tulolähteen ja yritysmuodon mukaan"
      ],
      correct: 0
    },
    {
      question: "23. Suomi voi aina verottaa myyntivoittoa, kun:",
      answers: [
        "a) Suom. yhtiö myy ulkomaisia osakkeita",
        "b) Ulkomainen yhtiö myy suoraan omistamansa kiinteistön Suomessa",
        "c) Ulkomainen yhtiö myy suomalaisen kiinteistöyhtiön osakkeet"
      ],
      correct: 1
    },
    {
      question: "24. Suomi ei voi yleensä verottaa toisessa EU-valtiossa asuvalle rajoitetusti verovelvolliselle maksettuja korkoja, koska:",
      answers: [
        "a) EU:n korko-rojaltidirektiivi estää sen",
        "b) Tuloverolaki estää sen",
        "c) Verosopimukset aina estävät sen"
      ],
      correct: 1
    },
    {
      question: "25. Suomessa yleisesti verovelvollisia ovat yhtiöt, joilla on:",
      answers: [
        "a) Vain kiinteä toimipaikka Suomessa",
        "b) Johtopaikka Suomessa (tai perustettu Suomeen)",
        "c) Enemmistöosakkaat suomalaisia"
      ],
      correct: 1
    },
    {
      question: "26. Hanna asuu Suomessa, työskenteli X, Y, Z -valtioissa ~14 kk. Työnantajan johtopaikka Suomessa. Y-valtion mekaanikkosääntö - Onko palkka veronalaista Suomessa?",
      answers: [
        "a) Koko palkka verovapaa",
        "b) Koko palkka veronalainen",
        "c) Vain Y:n palkka verovapaa, muu veronalainen"
      ],
      correct: 1
    },
    {
      question: "27. Verosopimuksen mukainen 'royalty' ei ole:",
      answers: [
        "a) Patentin käyttöoikeuskorvaus",
        "b) Tavaramerkin myyntihinta",
        "c) Salaisen kaavan käyttöoikeuskorvaus"
      ],
      correct: 1
    },
    {
      question: "28. Seuraava EU-direktiivi koskee osinkojen verotusta:",
      answers: [
        "a) EU:n yritysjärjestelydirektiivi",
        "b) EU:n emo-tytäryhtiödirektiivi",
        "c) EU:n voitonjakodirektiivi"
      ],
      correct: 1
    },
    {
      question: "29. A Oy (25 % B:stä, 20 % C:stä), B Oy (20 % X:stä), C Oy (50 % X:stä). Kenen väliyhteisö on X?",
      answers: [
        "a) Vain A",
        "b) Vain B",
        "c) Vain C",
        "d) A ja B",
        "e) B ja C",
        "f) A ja C",
        "g) Kaikkien (A, B, C)"
      ],
      correct: 2  // = 'c) Vain C'
    },
    {
      question: "30. EU-direktiivit säätelevät nimenomaisesti verotusta seuraavien tulojen osalta:",
      answers: [
        "a) Yhteisön ja luonnollisen henkilön korot ja osingot, muttei myyntivoitot",
        "b) Kaikki korot, osingot ja myyntivoitot (myös yksityishenkilöiden)",
        "c) Lähinnä yhteisöjen korot, osingot ja tietyt rajat ylittävät rojaltit"
      ],
      correct: 0
    }
  ];
  
  let currentQuestionIndex = 0;
  let scoreCount = 0;
  let usedIndices = [];

  const startBtn = document.getElementById('startBtn');
  const quizDiv = document.getElementById('quiz');
  const questionDiv = document.getElementById('question');
  const answersDiv = document.getElementById('answers');
  const checkBtn = document.getElementById('checkBtn');
  const feedbackDiv = document.getElementById('feedback');
  const nextBtn = document.getElementById('nextBtn');
  const summaryDiv = document.getElementById('summary');
  const scoreP = document.getElementById('score');

  // Alustetaan satunnainen järjestys
  let randomizedIndices = [...Array(questions.length).keys()];
  // Sekoitetaan taulukko (Fisher-Yates)
  for(let i = randomizedIndices.length - 1; i > 0; i--){
    const j = Math.floor(Math.random() * (i + 1));
    [randomizedIndices[i], randomizedIndices[j]] = [randomizedIndices[j], randomizedIndices[i]];
  }

  function showQuestion(index) {
    const qObject = questions[randomizedIndices[index]];
    questionDiv.textContent = qObject.question;
    
    // Tyhjennetään edelliset vastaukset
    answersDiv.innerHTML = "";
    feedbackDiv.textContent = "";
    feedbackDiv.classList.remove('correct', 'incorrect');
    nextBtn.classList.add('hidden');

    // Luodaan radio-napit
    qObject.answers.forEach((ans, i) => {
      const label = document.createElement('label');
      const radio = document.createElement('input');
      radio.type = 'radio';
      radio.name = 'answer';
      radio.value = i;
      label.appendChild(radio);
      label.appendChild(document.createTextNode(ans));
      answersDiv.appendChild(label);
      answersDiv.appendChild(document.createElement('br'));
    });
  }

  function checkAnswer() {
    const selected = document.querySelector('input[name="answer"]:checked');
    if(!selected){
      feedbackDiv.textContent = "Valitse ensin vastaus.";
      return;
    }
    const userAnswer = parseInt(selected.value);
    const correctAnswer = questions[randomizedIndices[currentQuestionIndex]].correct;
    
    if(userAnswer === correctAnswer){
      feedbackDiv.textContent = "Oikein!";
      feedbackDiv.style.color = "green";
      scoreCount++;
    } else {
      feedbackDiv.textContent = "Väärin!";
      feedbackDiv.style.color = "red";
    }
    nextBtn.classList.remove('hidden');
  }

  function nextQuestion() {
    currentQuestionIndex++;
    if(currentQuestionIndex < questions.length){
      showQuestion(currentQuestionIndex);
    } else {
      endQuiz();
    }
  }

  function endQuiz() {
    quizDiv.classList.add('hidden');
    summaryDiv.classList.remove('hidden');
    scoreP.textContent = `Sait ${scoreCount} / ${questions.length} pistettä.`;
  }

  startBtn.addEventListener('click', () => {
    startBtn.classList.add('hidden');
    quizDiv.classList.remove('hidden');
    showQuestion(currentQuestionIndex);
  });
  
  checkBtn.addEventListener('click', checkAnswer);
  nextBtn.addEventListener('click', nextQuestion);
</script>
</body>
</html>
