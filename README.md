# Harry Potter OWL Ontology

An OWL ontology modelling the Harry Potter universe, written in **Manchester Syntax** (`.omn`). Built as a group project for the **Semantic Web** course at DHBW Stuttgart.

The ontology covers characters, spells, potions, magical objects, locations, secret societies, and the relationships between them. A reasoner (e.g. HermiT) is required to infer a significant portion of the class memberships and relationships.

---

## Files

| File | Description |
|------|-------------|
| `harry_potter_ontology.omn` | OWL ontology in Manchester Syntax (~4 300 lines) |
| `characters.csv` | Source data used to populate character individuals (~360 rows) |

---

## Ontology Overview

### Classes (37)

| Category | Classes |
|----------|---------|
| **Beings** | `LivingBeing`, `Person`, `Wizard`, `Muggle`, `MuggleBorn`, `HalfBlood`, `PureBlood`, `Student`, `Professor`, `Ghost`, `Spirit`, `Animagus`, `Werewolf`, `Dementor`, `HouseElf`, `Goblin`, `Beast`, `Being` |
| **Magic** | `Spell`, `Charm`, `Curse`, `UnforgivableCurse`, `Counterspell`, `Potion` |
| **Objects** | `MagicalObject`, `Wand`, `Horcrux`, `DeathlyHallow`, `Broomstick`, `MagicalVehicle` |
| **Places & Groups** | `MagicalSchool`, `House`, `Location`, `SecretSociety`, `Class` |
| **Creatures** | `MagicalCreature` |

### Object Properties (34)

| Property | Domain → Range |
|----------|---------------|
| `hasParent` / `hasChild` / `hasFather` / `hasMother` | Person ↔ Person |
| `hasSibling` / `hasCousin` / `isAuntOrUncleOf` / `isNieceOrNephewOf` | Person ↔ Person |
| `isGrandparentOf` / `isGrandchildOf` | Person ↔ Person |
| `isMarriedTo` | Person ↔ Person |
| `isFriendOf` / `isEnemyOf` / `attacks` | LivingBeing ↔ LivingBeing |
| `isMemberOf` / `hasMember` | — ↔ SecretSociety / House |
| `isHeadOf` / `isPrefectOf` | Person → House |
| `isProfessorAtSchool` / `isStudentAtSchool` / `attends` | Person / Student → MagicalSchool / Class |
| `teaches` | Professor → Class |
| `performsSpell` / `uses` / `brews` | Wizard → Spell / Potion |
| `counters` / `destroys` / `killed` | — |
| `owns` / `hasPet` | Person → MagicalObject / Beast |
| `hasEnchanted` / `isLocatedIn` / `transformsInto` | — |

### Data Properties (4)

| Property | Type | Notes |
|----------|------|-------|
| `birthYear` | `xsd:integer` | Year of birth |
| `housePoints` | `xsd:integer` | Points awarded to a house |
| `incantation` | `xsd:string` | Spell incantation text |
| `effectDuration` | `xsd:decimal` | Duration of a potion's effect |

### Individuals (~255)

Covering major and minor characters, Hogwarts houses, spells, potions, locations, Horcruxes, Deathly Hallows, secret societies, and more. Notable examples:

- **Characters:** Harry Potter, Hermione Granger, Ron Weasley, Voldemort, Dumbledore, Draco Malfoy, Minerva McGonagall, and ~250 others sourced from the CSV
- **Spells:** Expelliarmus, Expecto Patronum, Avada Kedavra, Crucio, Imperio, Lumos, Nox, Alohomora, and more
- **Potions:** Veritaserum (and others with `effectDuration`)
- **Objects:** Elder Wand, Cloak of Invisibility, Resurrection Stone (Deathly Hallows); Tom Riddle's diary, Nagini, the Horcrux locket, etc.
- **Groups:** Gryffindor, Slytherin, Ravenclaw, Hufflepuff; Order of the Phoenix, Death Eaters

### SWRL Rules (5)

Automated inference rules:

| Rule | Effect |
|------|--------|
| Order of the Phoenix member + Death Eater member | → `isEnemyOf` |
| `hasParent(x, p)` + `hasParent(p, g)` | → `isGrandchildOf(x, g)` |
| Shared parent + `DifferentFrom` | → `hasSibling` |
| Parent's sibling | → `isAuntOrUncleOf` |
| Parents are siblings + `DifferentFrom` | → `hasCousin` |

---

## Usage

Open `harry_potter_ontology.omn` in [Protégé](https://protege.stanford.edu/) (version 5+) and run the **HermiT** or **Pellet** reasoner to infer:

- Grandparent/grandchild chains from `hasParent` assertions
- Sibling and cousin relationships via shared parents
- Aunt/uncle and niece/nephew relationships
- Enmity between Order of the Phoenix and Death Eater members
- Animagus form of McGonagall (`transformsInto Cat`)

---

## Course

Group project — **Semantic Web** course, DHBW Stuttgart.
