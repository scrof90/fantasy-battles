<script>
  let job = "";
  let enemy = "";

  const classes = {
    Wizard: {
      description: "Important stat: Charisma.",
      dis: "very stupid.",
      damage: () => {
        return 2 * getRandomInt(8) + 3;
      },
      hp: 20,
    },
    Sorcerer: {
      description: "Important stat: Intelligence.",
      dis: "very ugly.",
      damage: () => {
        return 1 * getRandomInt(12) + 4;
      },
      hp: 15,
    },
    Bard: {
      description: "Important stat: Dick Size.",
      dis: "very handsome.",
      damage: () => {
        return 5 * getRandomInt(4) + 1;
      },
      hp: 50,
    },
    Goblin: {
      description: "Noble son of caves and dungeons.",
      dis: "truly marvelous creature full of grace and pride.",
      damage: () => {
        return 2 * getRandomInt(6) + 2;
      },
      hp: 30,
    },
  };

  function getAdventurer(job) {
    let adventurer = Object.assign({}, classes[job]);
    adventurer["job"] = job;
    return adventurer;
  }

  function battle(adventurer, enemy) {
    const turns = [
      "You miss your first strike because you are a fucking clown.",
    ];

    while (true) {
      if (enemy.hp <= 0) {
        turns.push(
          `${enemy.job} has fallen in battle. You should be ashamed of yourself.`
        );
        break;
      } else {
        const enemyDamage = enemy.damage();
        adventurer.hp -= enemyDamage;
        turns.push(
          `${enemy.job} strikes you for ${enemyDamage}. You have ${adventurer.hp} HP.`
        );
      }

      if (adventurer.hp <= 0) {
        turns.push("You died.");
        break;
      } else {
        const adventurerDamage = adventurer.damage();
        enemy.hp -= adventurer.damage();
        turns.push(
          `You strike the ${enemy.job} for ${adventurerDamage}. ${enemy.job} has ${enemy.hp} HP.`
        );
      }
    }

    return turns;
  }

  function getRandomInt(max) {
    return Math.floor(Math.random() * max);
  }
</script>

<h1>What is your class?</h1>

{#each Object.keys(classes) as name}
  <label>
    <input type="radio" bind:group={job} name="job" value={name} />
    {name}
  </label>
{/each}

<h1>Who is your enemy?</h1>

{#each Object.keys(classes) as name}
  <label>
    <input type="radio" bind:group={enemy} name="enemy" value={name} />
    {name}
  </label>
{/each}

{#if job != "" && enemy != ""}
  <h2>You are a {job}!</h2>
  <p>
    {classes[job].description} You have {classes[job].hp} health points.
  </p>

  <h2>
    You encounter a {enemy}!
  </h2>
  <p>
    {enemy} is a {classes[enemy].dis}
  </p>
  <p>
    Because of your disgusting nature you attack the peaceful {enemy}! The
    battle ensues.
  </p>
  <ol>
    {#each battle(getAdventurer(job), getAdventurer(enemy)) as turn}
      <li>{turn}</li>
    {/each}
  </ol>
{/if}
