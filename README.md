# Fun-website
Interactive and engaging
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Random Fun Facts</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            padding: 60px 40px;
            max-width: 600px;
            width: 100%;
            text-align: center;
        }

        h1 {
            color: #667eea;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            color: #888;
            margin-bottom: 40px;
            font-size: 1.1em;
        }

        .fact-box {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 40px;
            border-radius: 15px;
            margin-bottom: 30px;
            min-height: 150px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.3em;
            line-height: 1.6;
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
        }

        .fact-text {
            font-weight: 500;
        }

        .button-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            flex-wrap: wrap;
        }

        button {
            padding: 15px 30px;
            font-size: 1em;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .btn-next {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-next:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.4);
        }

        .btn-next:active {
            transform: translateY(0);
        }

        .btn-copy {
            background: #f0f0f0;
            color: #333;
            border: 2px solid #667eea;
        }

        .btn-copy:hover {
            background: #667eea;
            color: white;
        }

        .fact-counter {
            color: #999;
            margin-top: 20px;
            font-size: 0.9em;
        }

        .loading {
            color: #667eea;
            font-style: italic;
        }

        @media (max-width: 600px) {
            .container {
                padding: 40px 25px;
            }

            h1 {
                font-size: 2em;
            }

            .fact-box {
                min-height: 120px;
                padding: 30px 20px;
                font-size: 1.1em;
            }

            button {
                padding: 12px 25px;
                font-size: 0.95em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎯 Fun Facts</h1>
        <p class="subtitle">Discover amazing facts about the world!</p>
        
        <div class="fact-box">
            <span class="fact-text" id="factText">Click "Get Fact" to start exploring!</span>
        </div>

        <div class="button-group">
            <button class="btn-next" onclick="getNewFact()">Get New Fact</button>
            <button class="btn-copy" onclick="copyFact()">Copy Fact</button>
        </div>

        <div class="fact-counter">
            <span id="counter">Facts viewed: 0</span>
        </div>
    </div>

    <script>
        // Array of fun facts
        const facts = [
            "Honey never spoils. Archaeologists have found pots of honey in ancient Egyptian tombs that are over 3,000 years old and still edible!",
            "A group of flamingos is called a 'flamboyance'. Imagine seeing a flamboyance of flamingos!",
            "Octopuses have three hearts: two pump blood to the gills, and one pumps blood to the rest of the body.",
            "Bananas are berries, but strawberries are not! Botanically speaking, berries have seeds on the inside.",
            "A day on Venus is longer than a year on Venus. Venus takes 243 Earth days to rotate on its axis but only 225 days to orbit the sun.",
            "Wombats produce cube-shaped poop. Scientists believe the shape prevents it from rolling away.",
            "Cleopatra lived closer to the invention of the iPhone than to the construction of the Great Pyramid.",
            "Sloths only defecate once a week. They lose up to 30% of their body weight when they do.",
            "A jiffy is an actual unit of time equal to one trillionth of a second.",
            "Carrots were originally purple, not orange. The orange ones were cultivated in the Netherlands in the 1600s.",
            "The smell of freshly cut grass is actually a distress signal released by the grass plant.",
            "Dolphins have names for each other and call out to individuals by their names.",
            "Scotland's national animal is a unicorn. It's been their national animal since the 12th century!",
            "Cows have best friends and get stressed when separated from them.",
            "A group of crows is called a 'murder'. A murder of crows is actually very intelligent!",
            "Butterflies taste with their feet. They have taste receptors on their feet to determine if plants are good to lay eggs on.",
            "The Great Wall of China isn't visible from space with the naked eye.",
            "Penguins have knees! They're hidden under their feathers and used for waddling.",
            "Cats spend 70% of their lives sleeping. That's 13-16 hours a day!",
            "Starfish don't have brains. They have a nerve net distributed throughout their body instead.",
            "A giraffe's tongue can be up to 20 inches long and is almost the same color as their body.",
            "Peacocks are actually male peafowls. Females are called peahens.",
            "Sharks have been around longer than dinosaurs. They've existed for over 400 million years.",
            "Hummingbirds can fly backwards. They're also the only birds that can do this.",
            "A cockroach can live for a week without its head before dying of starvation.",
            "Polar bears have black skin. Their white fur is actually translucent and reflects light.",
            "Koalas have fingerprints that are almost identical to human fingerprints.",
            "A group of hedgehogs is called a 'prickle'.",
            "Otters hold hands while sleeping so they don't drift apart. This is incredibly adorable!",
            "Mantis shrimp can see ultraviolet, visible, and polarized light, giving them the most complex color vision in the animal kingdom."
        ];

        let currentFactIndex = -1;
        let factsViewed = 0;

        function getNewFact() {
            let randomIndex;
            do {
                randomIndex = Math.floor(Math.random() * facts.length);
            } while (randomIndex === currentFactIndex && facts.length > 1);

            currentFactIndex = randomIndex;
            factsViewed++;

            const factText = document.getElementById('factText');
            factText.textContent = facts[randomIndex];
            document.getElementById('counter').textContent = `Facts viewed: ${factsViewed}`;
        }

        function copyFact() {
            if (currentFactIndex === -1) {
                alert('Please get a fact first!');
                return;
            }
            const factText = facts[currentFactIndex];
            navigator.clipboard.writeText(factText).then(() => {
                alert('Fact copied to clipboard!');
            }).catch(() => {
                alert('Failed to copy fact');
            });
        }

        // Load first fact on page load
        window.addEventListener('load', getNewFact);
    </script>
</body>
</html>
