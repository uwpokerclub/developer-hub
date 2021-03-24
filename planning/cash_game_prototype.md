## Engine interfaces

- shuffle the deck
- should deal cards to each player (amount given to engine)
- should handle adding to the pot
- should handle dealing N streets
- should know when a street is finished (and go to next)
- should determine the winning hand at the last street (based on passed in evaluators)
- should send all actions that happen in a hand (at the end) to a discord channel using a webhook
```ts
class Engine {
}

interface Game {
    evaluator: Evaluator,
    actions: Action[],
    deck: Deck,
    streets: Street[],
    blinds: Blind[],
    deal_inactive: boolean,
    allow_open_folding: boolean
}

interface Deck {
    cards: Card[]
}

interface Card {
    name: string,
    value: integer,
    suit: string
}

interface Evaluator {
    static final hand_ranks: List[Map{
        label: string,
        value: integer
        evaluator: callable => HandStrength
    }];
    evaluate_hands: callable(hands: PlayerHand[], board: Card[]) => Player[]
}

interface HandStrength {
    strength: integer[]
}

interface Action {
  type: string,
  perform: callable
}

interface Street {
  name: string;
  numberOfCards: integer;
}

- position is integer or integer[] which means one person or multiple ppl must pay it
interface Blind {
  name: string,
  value: integer,
  position: integer, integer[]
}

- Tournament level
interface Level {
  ante: integer;
  small: integer;
  big: integer;
}
```

## Table interfaces

```ts
interface Table {
    game: Game,
    players: Player[],
}

interface Player {
  stackSize: float,
  position: integer,
  active: boolean
}

interface TableHand {
    table: Table,
    pot: float,
    players: Player[],
    street: Street,
    bets: float[],
    action: Player,
    board: Card[],
    winner: Player[],
    actions: string[]
}

interface PlayerHand {
    hand: TableHand,
    player: Player,
    cards: Card[],
    strength: label
}
```