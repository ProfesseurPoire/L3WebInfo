/!\ Rectification : Seul les tps 1 à 3 seront notés. Pas de TP en plus pour la journée du 30 janvier.

Aide pour le tp 2, uniquement sur la partie filtre : 

```js
// Ici on remarque que le main component communique avec le component library à l'aide
// de la propriété personnalisée "filters". Et qu'on utilise ":" pour lier la
// valeur de l'attribut à une variable de notre component.
let mainComponent = app.component("main_component",
{
    template: '\
        <form id="form"> \
            <filters @filters_changed=updateFilters></filters> \
        </form>\
        <library :filters="myFilters"></library> ',
    methods: {
        updateFilters(newFilters) {
            this.myFilters = newFilters;
        }
    }
    // etc
```

```js
let library = app.component("library",
{
    // Dans le template, on peut remarquer l'utilisation de : qui permet de lier la valeur
    // de l'attribut à une variable de notre component.
    // Attention : il n'est pas possible de combiner v-for et v-if ensemble. D'ou l'utilisation
    // d'une fonction qui va retourner la liste d'objets à afficher filtrée et triée.
    template:
        '<ul>\
            <li v-for="item in getItems()" :class="item.class">\
                <img width="100" :src="item.image">\
                {{filters}}\
                <div>\
                    <p>{{ item.title }}</p>\
                </div>\
            </li>\
        </ul>'
    ,
    methods: {
        getItems() {
            // Fonction qui retourne un tableau d'élément filtrés.
        }
    },
    props: ["filters"]
    // etc
```


```js
let filters = app.component("filters",
{
    // Ici on remarque qu'on connecte l'événement change des checboxe à la fonction
    // updateFilters de notre component qui va ensuite émettre un signal.
    template: '<p id="Filters">Filtres :</p>\
        <div id="players">\
        Players\
        <div>\
        <input  id="filter_multi" v-model="multi_filter" @change="updateFilters()" type="checkbox">\
        <span>Multijoueur </span>\
    </div>\
    <div>\
        <input id="filter_solo" v-model="solo_filter" @change="updateFilters()" type="checkbox">\
        <span>Solo </span>\
    </div>\
</div> ',
    data: function () {
        return {
            multi_filter: true,
            solo_filter: true
        }
    },
    methods: {
        updateFilters() {
            let filt = "";
            if (this.solo_filter) {
                filt += "solo";
            }

            if (this.multi_filter) {
                filt += " multi";
            }
            // On déclenche l'événement "filters_changed" qui sera récupéré par le main component 
            this.$emit("filters_changed", filt);
        }
    }
}
)
```