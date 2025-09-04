# Summary: Pokémon Battle Mechanics Research

## Key Findings
This document analyzes the core mechanics of the Pokémon battle system, identifying key design principles that create its compelling strategic gameplay, and suggests how these can be adapted for the "cider vs orc beer" battle system.

-   **Turn-based Combat**: Priority system (moves with different priorities), Speed stat, and damage calculation formula (with 85-100% variance) create tactical depth and risk assessment.
-   **Type Effectiveness**: An 18x18 type chart with damage multipliers (0x to 4x) forces team diversity, strategic switching, and the use of "coverage moves" (attacks outside a Pokémon's type).
-   **Stats Specialization**: Six core stats (HP, Attack, Defense, Special Attack, Special Defense, Speed) define roles. Hidden layers (Individual Values, Effort Values, Natures) allow deep customization. Stat stages (+/- 6) enable dynamic setup strategies.
-   **Move Categories**: Physical, Special, and Status moves create asymmetric engagements. The four-move limit forces strategic choices between STAB (Same-Type Attack Bonus), coverage, and utility moves.
-   **Status Effects**: Major conditions (Sleep, Paralysis, Burn) and secondary effects (flinch, stat drops) fundamentally alter battle dynamics. Entry hazards (e.g., Stealth Rock) create persistent board control.
-   **Switching Mechanics**: Switching before attacks creates mind games and prediction loops. Pivot moves (U-turn), abilities (Intimidate), and Pursuit add layers of strategy.
-   **Team Building**: Demands synergistic construction with diverse roles (sweepers, walls, pivots, support) and win conditions (hyper offense, balance, stall).
-   **Evolution Systems**: Provide mechanical progression and emotional milestones (e.g., Magikarp to Gyarados). Mega Evolutions add temporary battle transformations.
-   **RNG**: Carefully balanced randomness (damage variance, critical hits, miss chances) adds excitement without undermining skill.
-   **Battle Flow**: Creates natural tension arcs through setup, trading, and endgame phases, with momentum shifts.
-   **Competitive Depth**: Emerges from the complex interactions of simple mechanics, leading to dynamic metagames and counter-strategies.

## Actionable Insights for System Design

1.  **Core Battle Loop**: Implement a turn-based combat system where "ciders" battle "orc beers." Define a "Fermentation Speed" stat for turn order, with some "cider abilities" having priority.
2.  **Flavor Profile "Types"**: Create a type effectiveness chart based on cider flavor profiles (e.g., Sweet, Dry, Tart, Complex, Funky) and orc beer styles (e.g., Hoppy, Malty, Sour, Dark, Wild). This will force strategic team composition and create meaningful matchups.
3.  **Cider "Stats"**: Define core stats for ciders, such as: 
    -   **Sweetness/Acidity**: Offensive power (physical/special attack equivalent) 
    -   **Complexity/Body**: Defensive power (physical/special defense equivalent) 
    -   **Alcohol Strength**: Speed/Priority 
    -   **Rarity**: HP/Staying power 
    -   **Carbonation**: Critical hit chance.
    Allow for customization through "aging" or "blending" (similar to IVs/EVs/Natures).
4.  **Four-Move Limit for Ciders**: Each cider can have a limited number of "abilities" or "attacks" (e.g., "Crisp Finish," "Tannic Bite," "Fruity Burst," "Wild Fermentation"). This forces strategic choices for movesets.
5.  **Status Effects & Hazards**: Introduce fermentation-based status effects (e.g., "Oxidation" for gradual stat decay, "Wild Yeast Infection" for confusion, "Brett Character" for type changes). Implement "Tasting Notes" as entry hazards that affect incoming ciders.
6.  **Strategic Switching**: Allow players to switch ciders during battle, creating mind games and requiring prediction. Implement "pivot" moves that deal damage and switch.
7.  **Synergistic Team Building**: Encourage players to build teams of ciders that complement each other's strengths and cover weaknesses. Define roles (e.g., "Offensive Sweet Cider," "Defensive Dry Cider").
8.  **Evolution/Aging Mechanics**: Implement a system where ciders can "evolve" or "age" (e.g., through logging more of them, or specific in-game actions) to gain new stats, abilities, or even change their "type" (flavor profile).
9.  **Balanced RNG**: Incorporate controlled randomness (e.g., damage variance, critical pour chances, secondary effect probabilities) to add excitement without making battles feel unfair.
10. **Battle Flow & Pacing**: Design the battle UI and animations to maintain engagement, with clear phases for setup, trading, and endgame.
11. **Integration with Rarity & Progression**: Link cider rarity to battle power or unique abilities. Ensure battle victories contribute to the "Warrior's Ring" progression.