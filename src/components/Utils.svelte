<script context="module">
  const ALL_TYPE = "all";

  export async function fetchPokemon(pokemonName) {
    try {
      const response = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${pokemonName}`
      );

      const data = await response.json();

      const pokemon = {
        name: data.name,
        id: data.id,
        types: data.types,
        wasFound: true,
        stats: data.stats,
        weight: data.weight,
        height: data.height,
      };

      if (pokemon.name === undefined || pokemon.id >= 10000) {
        return null;
      }

      return pokemon;
    } catch (error) {
      console.log(error);
      return null;
    }
  }

  export async function fetchPokemonDamageRelations(types) {
    async function x(names) {
      let doubleDamageTo = [];
      let doubleDamageFrom = [];

      for (let name of names) {
        const response = await fetch(
          `https://pokeapi.co/api/v2/type/${name.type.name}`
        );

        const data = await response.json();

        doubleDamageTo = [
          ...doubleDamageTo,
          ...data.damage_relations.double_damage_to,
        ];
        doubleDamageFrom = [
          ...doubleDamageFrom,
          ...data.damage_relations.double_damage_from,
        ];
      }

      return [doubleDamageTo, doubleDamageFrom];
    }

    try {
      return x(types);
    } catch (error) {
      console.log(error);
      return null;
    }
  }

  export async function fetchPokemonTypes() {
    try {
      const response = await fetch("https://pokeapi.co/api/v2/type/");
      const data = await response.json();
      const results = data.results;
      return results;
    } catch (error) {
      console.log(error);
      return null;
    }
  }

  export function mergeTypes(allPokemons, index = 1, max = 894) {
    if (index >= max) {
      return allPokemons.filter((element) => element.id < 999);
    } else {
      const isPokemonFilter = allPokemons.filter(
        (element) => element.id === index
      );

      if (isPokemonFilter.length === 1) {
        return mergeTypes(allPokemons, index + 1);
      } else {
        isPokemonFilter[0].types = [
          ...isPokemonFilter[0].types,
          ...isPokemonFilter[1].types,
        ];

        const allPokemonsFilter = allPokemons.filter(
          (element) => element.id !== index
        );

        return mergeTypes(
          [...allPokemonsFilter, isPokemonFilter[0]],
          index + 1
        );
      }
    }
  }

  export async function fetchByTypes(types, pokemonsByTypes = [], index = 0) {
    if (index >= types.length) {
      return pokemonsByTypes;
    } else {
      const response = await fetch(types[index].url);
      const data = await response.json();
      // const { data } = []

      const newPokemonsByTypes = getPokemon(data.name, data.pokemon, []);

      return fetchByTypes(
        types,
        [...newPokemonsByTypes, ...pokemonsByTypes],
        index + 1
      );
      // return null
    }
  }

  export function getPokemon(type, pokemons, newPokemons = [], index = 0) {
    if (index >= pokemons.length) {
      return newPokemons;
    } else {
      const newType = { name: type };

      const pokemon = {
        name: pokemons[index].pokemon.name,
        id: getId(pokemons[index].pokemon.url),
        types: [{ type: newType }],
      };
      return getPokemon(type, pokemons, [...newPokemons, pokemon], index + 1);
    }
  }

  export function getId(url) {
    const substr1 = "https://pokeapi.co/api/v2/pokemon/";
    const substr2 = "/";

    const newURL = replaceURL(url, [substr1, substr2]);

    const id = parseInt(newURL);
    return id;
  }

  export function replaceURL(URL, substr, index = 0) {
    if (index >= substr.length) {
      return URL;
    }
    const newURL = URL.replace(substr[index], "");
    return replaceURL(newURL, substr, index + 1);
  }

  export function formatData(pokemonStats) {
    const axisX = pokemonStats.map((element) =>
      element.stat.name.replace("-", " ")
    );

    const data = pokemonStats.map((element) => element.base_stat);

    return [data, axisX];
  }

  export function formatWeight(value) {
    let str = value.toString();
    if (str.length === 1) {
      str = "0".concat(value.toString());
    } else {
      str = value.toString();
    }
    return [
      str.slice(0, str.length - 1),
      ",",
      str.slice(str.length - 1),
      " kg",
    ].join("");
  }

  export function formatHeight(value) {
    let str = value.toString();
    if (str.length === 1) {
      str = "0".concat(value.toString());
    } else {
      str = value.toString();
    }
    return [
      str.slice(0, str.length - 1),
      ",",
      str.slice(str.length - 1),
      " m",
    ].join("");
  }

  export function getDamageRelation(
    pokemonType,
    doubleDamageTo,
    doubleDamageFrom
  ) {
    function filterSameTypes(relation, pokemonType, index = 0) {
      if (index >= pokemonType.length || pokemonType.length === 1) {
        return relation;
      } else {
        const found = relation.filter(
          (item) => item.name !== pokemonType[index].type.name
        );

        return filterSameTypes(found, pokemonType, index + 1);
      }
    }

    const advantage = filterSameTypes(doubleDamageTo, pokemonType);
    const weakness = filterSameTypes(doubleDamageFrom, pokemonType);

    const newAdvantage = advantage.filter((element) =>
      filterRelation(element.name, weakness)
    );
    const newWeakness = weakness.filter((element) =>
      filterRelation(element.name, advantage)
    );

    return [newAdvantage, newWeakness];
  }

  export function filterRelation(estatico, arr) {
    for (let { name } of arr) {
      if (name !== estatico) {
        return true;
      } else {
        return false;
      }
    }
  }

  export function applyAllFilter(
    pokemonsFilter,
    pokemons,
    name,
    type,
    offset,
    limit
  ) {
    let x;

    if (name !== "" && name.length > 2) {
      x = pokemons.filter((element) =>
        element.name.includes(name.toLowerCase())
      );

      if (type !== ALL_TYPE) {
        const v = x.filter((element) => filterTypes(element.types, type));
        return v;
      }

      return x;
    }

    if (name === "") {
      x = pokemons;

      if (type !== ALL_TYPE) {
        const v = x.filter((element) => filterTypes(element.types, type));
        return v;
      }

      return x.slice(offset, limit);
    }

    return pokemonsFilter;
  }

  export function filterTypes(types, type) {
    for (let x of types) {
      if (x.type.name === type) {
        return true;
      }
    }

    return false;
  }

  export const sortLessToGreater = (prev, next) => {
    if (prev.id > next.id) {
      return 1;
    }
    if (prev.id < next.id) {
      return -1;
    }
    return 0;
  };

  // const response = fetch(types[index].url);

  // const URL = "type/normal"
  // fetch(`https://pokeapi.co/api/v2/${URL}`)
  //   .then((response) => response.json())
  //   .then((data) => console.log(data));

  // const getAllPokemonsFilter = () =>
  //   applyAllFilter(
  //     pokemonsFilter,
  //     pokemons,
  //     pokemonSearches,
  //     type,
  //     offset,
  //     limit
  //   );

  // (async () => {
  //   pokemons = mergeTypes(await fetchByTypes(types, [])).sort(
  //     sortLessToGreater
  //   );
  // })();
</script>
