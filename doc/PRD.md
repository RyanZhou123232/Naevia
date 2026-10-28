# Naevia — Product Requirements

Version: 0.4 | Status: Product direction and local Demo 1 scope

## 1\. Overview

Naevia is a companionship-focused pixel game set in a private world.

Players gather materials, furnish a home, and spend time with AI characters who have distinct personalities, lives, and memories. Characters naturally enter the player's daily activities, share their own news, and respond to the player's experiences.

Over time, real-life moments can become keepsakes. The home grows into a personal record of life and the relationships formed along the way.

## 

## 2\. Player Pain Points

These are product hypotheses to validate through playtesting.

### 2.1 Wanting to relax while doing something

Players may want simple activities with visible progress, without competition, difficult decisions, or urgent objectives.

### 2.2 Wanting company without a dedicated chat session

Players may want to talk while doing something else. Starting every conversation and constantly finding topics can feel like work.

### 2.3 Wanting everyday experiences to be remembered

Small accomplishments and meaningful moments are easily forgotten. They may matter deeply without being something the player wants to share publicly.

### 2.4 Wanting care and privacy

Players want natural attention and encouragement, with control over when and with whom they share. Constant praise and characters knowing things they were never told can undermine that experience.



## 3\. How Naevia Responds

### 3.1 Low-pressure activities

Gathering, making things, decorating, and eventually expanding the home give players something satisfying to do. Activities are easy to start and stop.

### 3.2 Company during everyday play

Characters reach out through visits, encounters, and messengers. Players can accept an invitation or initiate contact themselves. Characters bring their own experiences and topics to the conversation.

### 3.3 A world that holds personal history

Players choose which experiences matter. Future keepsakes connect those experiences to objects that can be displayed and revisited.

### 3.4 Relationships with distinct perspectives

Each character remembers their own experiences. Information can spread through conversations characters actually participate in, rather than through a shared omniscient memory.



## 4\. Core Activities

### 4.1 Gather, make, and build

Players gather materials through simple activities such as mining. Materials can support furniture, decorations, selected keepsakes, and future home or courtyard expansion.

### 4.2 Interact with AI characters

**Everyday contact:** A witch's pigeon arrives while the player is mining, or a blacksmith stops by the house. Clicking the messenger or character opens a conversation. Invitations can be ignored without penalty.

**Direct and group contact:** Players can approach a character or invite multiple characters to share news. Characters can speak to the player and to each other.

**Special occasions:** Future versions may include celebrations for player-selected birthdays and anniversaries. Characters may prepare decorations, messages, or gifts. Official gifts, potentially including gift cards, remain an optional future idea.

Conversations appear as in-world bubbles. Replies are usually one to three sentences; longer replies are split into readable segments. Encouragement should follow the character and situation, without constant flattery.

### 4.3 Record life and create keepsakes

The long-term product lets players preserve experiences as collectible objects. Some objects may require gathered materials. The specific creation flow is deferred beyond Demo 1.

### 4.4 Decorate and revisit

Players arrange furniture and collectibles to shape their home. Future keepsakes retain the stories and dates behind them and can become starting points for conversation.

## 

## 5\. Characters, Relationships, and Privacy

The world belongs to the player; it does not need to be a town or a public community. Characters have lives elsewhere and connect through visits, messages, and conversation.

Memory is handled automatically in the background. Players do not need to say “remember this” or approve routine memory updates.

Each character has independent memory. They learn through direct conversations, observable events, and exchanges they hear. Characters may ask each other questions and remember the answers. Private knowledge does not automatically become group knowledge.

For example, the witch knows the player has been studying for exams. The blacksmith does not. During a gathering, the player announces making the Dean's List. The blacksmith can ask about it and learn through the discussion.

There are no relationship penalties for declining invitations or taking time away. Game-world privacy and the data sent to an AI provider are separate concerns and must be explained accurately when the product is released.



## 6\. Long-Term Experience

* A home that increasingly reflects the player's life and choices.
* Familiar relationships built from remembered details and shared experiences.
* A personal history accumulated through ordinary days and meaningful moments.
* A welcoming place to return to, without attendance pressure.



## 7\. Demo 1

Build a small, playable foundation:

* Windows desktop game with a side-view 2D pixel world.
* Game client, backend, and saves run on the same local computer. No accounts, cloud saves, or hosted backend are required for Demo 1. Live AI still uses a configured cloud provider and requires an internet connection; mock mode supports offline development.
* One home and a small gathering area.
* Movement, jumping, simple mining, and basic furniture placement.
* Player, witch, blacksmith, and pigeon visuals, with essential animation.
* Cloud AI conversations presented through clickable, adaptive bubbles.
* Automatic, separate character memories across sessions.
* A simple invitation for a player–witch–blacksmith conversation, including character-to-character exchanges.
* Saved world, inventory, furniture, conversations, and memories.

Demo 1 excludes keepsake creation, home expansion, special-date events, official gifts, and local model deployment. It also excludes combat, competition, multiplayer, and a large open world.



## 8\. What Demo 1 Should Validate

* Light activities and conversation feel comfortable together.
* Characters feel distinct and have their own lives.
* Characters remember relevant details without knowing everything.
* Group conversations allow information to spread naturally.
* Players want to return to the space and its characters.



## 9\. Next Step

Use [SPEC.md](SPEC.md) to build the Demo 1 foundation. Product and system documentation lives in `doc/`. Refine detailed rules through discussion and playtesting before expanding the feature set.

