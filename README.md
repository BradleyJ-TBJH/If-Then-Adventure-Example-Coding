# If-Then-Adventure-Example-Coding
Example of the capability of creating an if-then adventure that involves a detailed storyline. 


Original code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dragon's Rescue - Choose Your Own Adventure</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
            max-width: 800px;
            width: 100%;
            padding: 40px;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        h1 {
            color: #764ba2;
            text-align: center;
            margin-bottom: 20px;
            font-size: 2.5em;
        }

        .image-placeholder {
            width: 100%;
            height: 300px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 18px;
            margin-bottom: 30px;
            font-weight: bold;
            text-align: center;
            padding: 20px;
        }

        .story-text {
            font-size: 18px;
            line-height: 1.8;
            color: #333;
            margin-bottom: 30px;
            text-align: justify;
        }

        .items-collected {
            background: #f0f4ff;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            border-left: 4px solid #667eea;
        }

        .items-collected h3 {
            color: #667eea;
            margin-bottom: 10px;
        }

        .items-list {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }

        .item-badge {
            background: #667eea;
            color: white;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 14px;
        }

        .choices {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 20px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
            text-align: left;
        }

        button:hover {
            transform: translateX(5px);
            box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
        }

        button:active {
            transform: translateX(2px);
        }

        .ending {
            background: #fff3cd;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #ffc107;
            margin-bottom: 20px;
        }

        .ending h2 {
            color: #856404;
            margin-bottom: 10px;
        }

        .ending p {
            color: #856404;
            line-height: 1.8;
        }

        .restart-btn {
            background: #28a745;
            width: 100%;
            margin-top: 20px;
        }

        .restart-btn:hover {
            background: #218838;
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🐉 Dragon's Rescue 🐉</h1>
        <div id="gameContent"></div>
    </div>

    <script>
        // ============================================
        // GAME STATE MANAGEMENT
        // ============================================
        const gameState = {
            currentScene: 'start',
            itemsCollected: [],
            choicesMade: []
        };

        // ============================================
        // STORY SCENES & BRANCHING LOGIC
        // ============================================
        const scenes = {
            start: {
                title: "Your Sister's Kidnapping",
                image: "A dark castle silhouette against a purple sky with lightning",
                text: "Your beloved sister has been kidnapped by the Purple Dragon and taken to her fortress deep in the Enchanted Forest. You stand at the edge of the forest, ready to rescue her. The forest path splits into three directions.",
                choices: [
                    { text: "🌲 Take the LEFT path through the dense trees", next: 'leftForest' },
                    { text: "🌲 Take the MIDDLE path along the crystal river", next: 'riverPath' },
                    { text: "🌲 Take the RIGHT path through the misty mountains", next: 'mountainPath' }
                ]
            },

            leftForest: {
                title: "The Deep Forest",
                image: "A magical forest with glowing trees and mysterious creatures",
                text: "As you venture deeper into the dense forest, you spot a glowing Fairy sitting on a mushroom. She looks distressed. 'Please help me!' she cries. 'A shadow creature has stolen my magic wand, and I can't help the forest anymore.'",
                choices: [
                    { text: "✨ Help the Fairy find her wand", next: 'helpFairy', item: 'Fairy\'s Wand' },
                    { text: "⏭️ Politely decline and continue your journey", next: 'afterLeftForest' }
                ]
            },

            helpFairy: {
                title: "The Fairy's Quest",
                image: "A magical wand glowing with purple and blue light",
                text: "The Fairy thanks you profusely and gives you her enchanted wand! 'This wand can illuminate dark places and reveal hidden paths. You'll need it where you're going.' You feel grateful for helping her.",
                choices: [
                    { text: "Continue forward", next: 'afterLeftForest' }
                ]
            },

            afterLeftForest: {
                title: "Forest Guardian",
                image: "A wise old owl perched on an ancient tree",
                text: "You encounter a wise Owl Guardian who blocks the path. 'To pass, you must answer my riddle: I have cities but no houses, forests but no trees, and water but no fish. What am I?' The answer is... a map!",
                choices: [
                    { text: "Answer: A map", next: 'owlCorrect', item: 'Ancient Map' },
                    { text: "Answer: A dream", next: 'owlWrong' },
                    { text: "Answer: Magic", next: 'owlWrong' }
                ]
            },

            owlCorrect: {
                title: "The Owl's Gift",
                image: "An ancient, glowing map with mysterious symbols",
                text: "The Owl smiles wisely and gives you an Ancient Map to the Dragon's Castle. 'You'll need this to navigate the final passage,' he says, stepping aside to let you pass.",
                choices: [
                    { text: "Thank the Owl and continue", next: 'dragonCastle' }
                ]
            },

            owlWrong: {
                title: "The Owl's Challenge",
                image: "The owl shakes its head",
                text: "The Owl shakes his head. 'That is incorrect, but I admire your courage. I will let you pass anyway, for bravery is worth more than riddles.' The Owl steps aside.",
                choices: [
                    { text: "Continue forward", next: 'dragonCastle' }
                ]
            },

            riverPath: {
                title: "The Crystal River",
                image: "A sparkling river with magical crystals reflecting light",
                text: "You follow the crystal river and encounter a sad Water Sprite sitting by the shore. 'My river is drying up because someone blocked the source upstream. Will you help me restore it?'",
                choices: [
                    { text: "🌊 Help the Water Sprite", next: 'helpSprite', item: 'Water Crystal' },
                    { text: "⏭️ Continue on your way", next: 'afterRiverPath' }
                ]
            },

            helpSprite: {
                title: "Restoring the River",
                image: "Water flowing beautifully through a magical landscape",
                text: "You work together to clear the blockage, and the river flows beautifully again! The grateful Water Sprite gives you a Water Crystal that can purify any dark magic. 'You have a kind heart,' she says.",
                choices: [
                    { text: "Continue forward", next: 'afterRiverPath' }
                ]
            },

            afterRiverPath: {
                title: "The Trickster Goblin",
                image: "A mischievous goblin with glowing eyes",
                text: "A Trickster Goblin appears! 'I can help you reach the castle quickly... OR I can send you in circles for hours. Choose wisely: Do you have any magical items to trade for safe passage?'",
                choices: [
                    { text: "Offer a magical item if you have one", next: 'goblinTrade', requiresItem: true },
                    { text: "Try to outsmart the Goblin with clever words", next: 'goblinTrick' },
                    { text: "Refuse and find another way", next: 'dragonCastle' }
                ]
            },

            goblinTrade: {
                title: "A Fair Trade",
                image: "The goblin smiling with satisfaction",
                text: "The Goblin accepts your item and creates a magical shortcut directly to the castle entrance. 'Smart move, friend!' he cackles as you zoom through the forest at lightning speed.",
                choices: [
                    { text: "Arrive at the Dragon's Castle", next: 'dragonCastle' }
                ]
            },

            goblinTrick: {
                title: "Outsmarting Mischief",
                image: "The goblin looking confused",
                text: "You tell the Goblin a clever riddle that confuses him so much he forgets about his tricks. Impressed by your wit, he lets you pass with a grudging nod.",
                choices: [
                    { text: "Continue to the castle", next: 'dragonCastle' }
                ]
            },

            mountainPath: {
                title: "The Misty Mountains",
                image: "Snow-capped mountains shrouded in purple mist",
                text: "You climb the misty mountains and find a Dragon's Egg hidden in a cave, still warm and glowing. A note says: 'This egg needs love and protection. If you care for it, it will help you when you need it most.'",
                choices: [
                    { text: "🥚 Take the egg and care for it", next: 'takeEgg', item: 'Dragon\'s Egg' },
                    { text: "Leave the egg safely in the cave", next: 'leaveEgg' }
                ]
            },

            takeEgg: {
                title: "A New Companion",
                image: "A glowing purple dragon egg in your hands",
                text: "You carefully carry the warm egg with you. As you travel, it glows brighter, as if responding to your kindness. You feel a special bond forming.",
                choices: [
                    { text: "Continue to the castle", next: 'mountainEnd' }
                ]
            },

            leaveEgg: {
                title: "Respecting Nature",
                image: "The egg glowing peacefully in the cave",
                text: "You decide to leave the egg in its safe cave, knowing it's better protected here. You continue your journey with a clear conscience.",
                choices: [
                    { text: "Continue to the castle", next: 'mountainEnd' }
                ]
            },

            mountainEnd: {
                title: "Peak of Courage",
                image: "A breathtaking view of the Dragon's Castle from the mountain peak",
                text: "From the mountain peak, you can finally see the Purple Dragon's Castle in the distance. Your heart races with determination. You begin your descent toward your sister's rescue.",
                choices: [
                    { text: "Head toward the castle", next: 'dragonCastle' }
                ]
            },

            dragonCastle: {
                title: "The Dragon's Fortress",
                image: "A grand purple castle with towers and magical aura",
                text: "You've reached the Dragon's Castle! The massive doors are guarded by the Purple Dragon herself. She's enormous, with scales that shimmer like amethyst. Your sister is visible in a high tower, waving! But the Dragon blocks your path, looking curious rather than angry.",
                choices: [
                    { text: "🎁 Offer a gift or magical item to negotiate", next: 'dragonNegotiate', requiresItem: true },
                    { text: "💬 Try to talk to the Dragon peacefully", next: 'dragonTalk' },
                    { text: "🏃 Try to sneak past the Dragon", next: 'dragonSneak' }
                ]
            },

            dragonNegotiate: {
                title: "A Dragon's Deal",
                image: "The Dragon's eyes glowing with interest",
                text: "You offer the magical item to the Dragon. Her eyes widen with surprise and joy! 'In all my years, no one has ever offered me a gift. Everyone just tries to fight or trick me.' She lowers her head thoughtfully. 'You may go to your sister. And tell her... I'm sorry for frightening her. I was just very lonely.'",
                choices: [
                    { text: "Rescue your sister", next: 'ending5' }
                ]
            },

            dragonTalk: {
                title: "Understanding the Dragon",
                image: "The Dragon listening intently to your words",
                text: "You speak to the Dragon with kindness and respect. 'I came to rescue my sister, but I can see you're not evil—just lonely and misunderstood.' The Dragon's fierce expression softens. Tears form in her eyes. 'You're right. I took your sister hoping for a friend, not a prisoner.'",
                choices: [
                    { text: "Continue", next: 'dragonTalkContinue' }
                ]
            },

            dragonTalkContinue: {
                title: "A New Understanding",
                image: "The Dragon and you connecting peacefully",
                text: "The Dragon releases your sister, and she runs to embrace you. The Dragon asks shyly, 'Will you visit me sometimes? I promise to be better.' You agree, and the Dragon smiles—a genuine, kind smile.",
                choices: [
                    { text: "Return home with your sister", next: 'ending4' }
                ]
            },

            dragonSneak: {
                title: "A Risky Move",
                image: "You creeping quietly past the Dragon",
                text: "You try to sneak past the Dragon, but she's too alert. She spots you immediately! However, instead of attacking, she looks hurt. 'You don't trust me? I would never hurt
            dragonSneak: {
                title: "A Risky Move",
                image: "You creeping quietly past the Dragon",
                text: "You try to sneak past the Dragon, but she's too alert. She spots you immediately! However, instead of attacking, she looks hurt. 'You don't trust me? I would never hurt your sister.' Ashamed, you apologize and she forgives you, letting you pass.",
                choices: [
                    { text: "Rescue your sister", next: 'ending3' }
                ]
            },

            ending1: {
                title: "🎉 ENDING 1: The Lonely Rescue",
                image: "You and your sister walking away from the castle together",
                text: "You rescued your sister, but the Dragon remains alone in her castle. She watches from the tower as you leave. Your sister is safe, but you can't help feeling a bit sad for the misunderstood creature you left behind. A bittersweet victory.",
                choices: [
                    { text: "Play Again", next: 'start' }
                ]
            },

            ending2: {
                title: "🎉 ENDING 2: The Brave Negotiator",
                image: "You shaking hands with the Dragon as your sister watches",
                text: "Your quick thinking and bravery saved the day! You negotiated with the Dragon using your wits, rescued your sister, and even made peace with the Dragon. She promises to change her ways. You leave as heroes, and the Dragon begins a new chapter of kindness.",
                choices: [
                    { text: "Play Again", next: 'start' }
                ]
            },

            ending3: {
                title: "🎉 ENDING 3: The Humble Hero",
                image: "You apologizing to the Dragon while your sister is freed",
                text: "Your willingness to admit your mistake and apologize touched the Dragon's heart. She releases your sister without hesitation. You leave with your sister safe AND with a newfound friend in the Dragon. She promises to visit your village soon—as a friend, not a threat.",
                choices: [
                    { text: "Play Again", next: 'start' }
                ]
            },

            ending4: {
                title: "🎉 ENDING 4: The Compassionate Rescuer",
                image: "You, your sister, and the Dragon together peacefully",
                text: "By listening to the Dragon's loneliness with genuine compassion, you didn't just rescue your sister—you changed a life. The Dragon becomes a trusted friend to your family. Years later, she's known throughout the land as the gentle guardian who protects travelers. Your kindness created an unexpected friendship that heals old wounds.",
                choices: [
                    { text: "Play Again", next: 'start' }
                ]
            },

            ending5: {
                title: "🎉 ENDING 5: The Gift of Friendship",
                image: "The Dragon accepting your gift with joy and gratitude",
                text: "Your generosity—giving the Dragon a precious magical item—was the greatest gift she'd ever received. Not just the object, but the recognition that she deserved kindness. She releases your sister joyfully and asks you to return often. You leave knowing you've made the world a better place by choosing compassion over conflict.",
                choices: [
                    { text: "Play Again", next: 'start' }
                ]
            }
        };

        // ============================================
        // GAME ENGINE FUNCTIONS
        // ============================================

        // Function to display the current scene
        function displayScene(sceneKey) {
            const scene = scenes[sceneKey];
            if (!scene) {
                console.error('Scene not found:', sceneKey);
                return;
            }

            gameState.currentScene = sceneKey;
            const gameContent = document.getElementById('gameContent');
            
            let html = `
                <div class="image-placeholder">${scene.image}</div>
                <h2>${scene.title}</h2>
                <p class="story-text">${scene.text}</p>
            `;

            // Display collected items
            if (gameState.itemsCollected.length > 0) {
                html += `
                    <div class="items-collected">
                        <h3>📦 Items You've Collected:</h3>
                        <div class="items-list">
                            ${gameState.itemsCollected.map(item => `<span class="item-badge">${item}</span>`).join('')}
                        </div>
                    </div>
                `;
            }

            // Display choices
            html += '<div class="choices">';
            scene.choices.forEach((choice, index) => {
                // Check if choice requires an item
                if (choice.requiresItem && gameState.itemsCollected.length === 0) {
                    html += `<button disabled style="opacity: 0.5; cursor: not-allowed;">${choice.text} (Need an item)</button>`;
                } else {
                    html += `<button onclick="makeChoice('${choice.next}', ${choice.item ? `'${choice.item}'` : 'null'})">${choice.text}</button>`;
                }
            });
            html += '</div>';

            gameContent.innerHTML = html;
        }

        // Function to handle player choices
        function makeChoice(nextScene, item) {
            if (item) {
                gameState.itemsCollected.push(item);
            }
            gameState.choicesMade.push(gameState.currentScene);
            displayScene(nextScene);
        }

        // ============================================
        // START THE GAME
        // ============================================
        displayScene('start');
    </script>
</body>
</html>
