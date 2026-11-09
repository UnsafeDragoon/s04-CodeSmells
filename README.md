# S04-CodeSmells

## Duplicated Code

### Explaination

Duplicated code according to Martin Fowler is a situation in which you see the same code snippet/structure repeated multiple times. They may not necessarily have to be the exact same, but if they're so similar that you second guess yourself that they aren't, then chances are you can conjoin them with another piece of code somehow.  
<br>
There are many situations in which duplicated code can arise. Refactoring Guru tells us that the most common reasons for similar code is when developers, particularly novice, crunch past what they are capable of and end up copy and pasting relevant code. Some developers are also flat-out lazy and don't bother with reviewing/cleaning up code they worked on. Another situation may be that a team has different members working on different areas of the program, and therefore are unable to know that each other may be working on similar solutions to existing problems.  
<br>
The main issue with duplicated code is that it over crowds the program to a size that it really doesn't need to be at all. It's essentially dead space that makes reading the program either more confusing for complex applications or extremely monotonous, flooding any new developers with unnecessary information. 
<br>

### Prevention and Refactoring

The solution you use heavily depends on the structure of the body of whatever the duplicated code is within. For example, if the line is repeated exact-for-exact and perhaps nets a required value, consider extracting a function from the repeat code and call it in its former locations. If it's similar but not quite there, utilize slide statements and re-arrange the code in an order that makes extraction possible. Particular to classes, but if you find two subclasses sharing the same initialization, it may be more reasonable to use the Pull Up Method and bring that initialization responsibility to the superclass.  
<br>
This issue can be avoided by simply reviewing code before leaving or committing a change. Ensure that you aren't repeating what was written before, or if it was, refactor it to make them shortened. Take your time when programming and don't rush yourself, and you'll find that you won't resort to hasty copying and pasting to meet deadlines.
<br>

### Code Example

Below is a snippet of code that I made back in high school when I was learning Web Development. This is for a web-game based off the board game Sorry!, programmed with HTML, Css, and Javascript. This code is quite ugly to just look at, and just by looking at it, you can tell that each section was copy and pasted. This is only one store item as well, but I won't show the rest, as that is it's own refactoring. 
<br>
This code is very repetitive in a way that is very distracting in a larger sense with the entire codebase (2000+ lines). It does not also help that the "coin system” is defined by an object PER color, which seems almost backwards. 



    let redCoins = {
    coins: 0
    };

    let yellowCoins = {
    coins: 0
    };

    // pearl
    item1.addEventListener('click', () => {
    if (greenCoins.coins >= 1 && currentColorTurn == 'green') {
        removeOverlay()
        removeStore()
        removecoinevent()
        greenItem = 'pearl'
    }
    else if (blueCoins.coins >= 1 && currentColorTurn == 'blue') {
        removeOverlay()
        removeStore()
        removecoinevent()
        blueItem = 'pearl'
    }
    else if (redCoins.coins >= 1 && currentColorTurn == 'red') {
        removeOverlay()
        removeStore()
        removecoinevent()
        redItem = 'pearl'
    }
    else if (yellowCoins.coins >= 1 && currentColorTurn == 'yellow') {
        removeOverlay()
        removeStore()
        removecoinevent()
        yellowItem = 'pearl'
    }
    })

As you can see below, even just changing the variable we use helps the structure a lot. Moreso, by using the extraction technique, we successfully ripped those three repetitive lines and pushed them to the top, as all 4 colors share this guaranteed execution. Furthermore, if we really wanted to, we could make an object for the items as well, removing the need for the conditional checks. 


    const coins = {
        green: 0,
        blue: 0,
        red: 0,
        yellow: 0,
    };

    if ( coins[currentColorTurn] >= 1) {
    removeOverlay()
            removeStore()
            removecoinevent()

        if ( currentColorTurn === 'green' ) greenItem = 'pearl';
        if ( currentColorTurn === 'blue' ) blueItem = 'pearl';
        if ( currentColorTurn === 'red' ) redItem = 'pearl';
        if ( currentColorTurn === 'yellow' ) yellowItem = 'pearl';
    }


## Long Function


### Explaination

Long function according to Fowler is a situation where a single function simply runs on for way too long, assumes way too many responsibilities, and is generally hard to comprehend its purpose. One subtle sign of a incoming long function in its infancy is excessive use of comments, which indicate that the function naming itself is not adequate enough to convey the purpose, suggesting multitasking. 
<br>
People often enjoy writing code much more than reading it, so this situation can happen when code goes unchecked for too long. A lack of extraction leads to an oversized utility that seems almost daunting.
<br>
Not only that, but these long scrolls of code can also hide other fundamental issues, such as heavy repetition, mysterious variables, and commonly comments. 
<br>

### Prevention and Refactoring

The solution you use heavily depends on the structure of the body of whatever the duplicated code is within. For example, if the line is repeated exact-for-exact and perhaps nets a required value, consider extracting a function from the repeat code and call it in its former locations. If it's similar but not quite there, utilize slide statements and re-arrange the code in an order that makes extraction possible. Particular to classes, but if you find two subclasses sharing the same initialization, it may be more reasonable to use the Pull Up Method and bring that initialization responsibility to the superclass. 
<br>
This issue can be avoided by simply reviewing code before leaving or committing a change. Ensure that you aren't repeating what was written before, or if it was, refactor it to make them shortened. Take your time when programming and don't rush yourself, and you'll find that you won't resort to hasty copying and pasting to meet deadlines.
<br>

### Code Example

This is a more recent program I made for a course here at UCSC. It is apart of a larger search engine application that splices sets against each other to filter search queries (ex. cats AND dogs ---- or ---- cats OR dogs), hence the union and intersection functions below. I'll only show the union code because they're slightly similar. 
<br>
This explanation makes it clear its purpose, but by looking directly at the function, it may not be as obvious.There are 4 distinct responsibilities here, and a strong smell from the comments. Fowler describes comments as a double edged sword, in that they are a "sweet smell" to hide rotten code. 
<br>
The four responsibilities are:
- Validating that the queries exist
- Handling the case of only a single query
- Initialize the result of two queries
- Recursively accumulate the rest of the sets
<br>

    string_set *union_lookup(TermLookupTable *table, ll_string *queries) {
    // YOUR CODE HERE
    
        if(!queries){
            printf(">>> HAVE 0 TERMS BUT NEED AT LEAST ONE!\n");
            return NULL;
        } 
        
        if(!queries->next){
            return set_union(
                    term_lookup(table, queries->value),
                    NULL);
        }

        // Form the base and then see if we need to go further or can leave as is.

        string_set *cumlativeTree = set_union(
                term_lookup(table, queries->value),
                term_lookup(table, queries->next->value)
                );


        if(!queries->next->next){
            return cumlativeTree;	
        }

        // If there exists three or more queries
        queries = queries->next->next;
        while(queries){
            string_set *newTree = term_lookup(table, queries->value);

            string_set *tmp = set_union(cumlativeTree, newTree);
            free_set(cumlativeTree);
            cumlativeTree = tmp;

            queries = queries->next;
        }

            return cumlativeTree;
    }

<br>
In the revised version below, you can see we applied the extraction principle to the while loop, establishing a clear responsibility to it as the accumulation function, whereas union_lookup performs necessary guardrails, and only then, calls 
<br>

    string_set *accumulate_union( string_set *cumulative, ll_string *queries, TermLookupTable *table ) {
        while (queries) {
            string_set *new_set = term_lookup(table, queries->value);

            string_set *tmp = set_union(cumulative, new_set);
            free_set(cumulative);

            cumulative = tmp;
            queries = queries->next;
        }

        return cumulative;
    }

    string_set *union_lookup( TermLookupTable *table, ll_string *queries ) {
        if (!queries) {
            printf(">>> HAVE 0 TERMS BUT NEED AT LEAST ONE!\n");
            return NULL;
        }

        if (!queries->next) {
            return set_union(
                term_lookup(table, queries->value),
                NULL
            );
        }

        string_set *cumulative = set_union( term_lookup(table, queries->value), term_lookup(table, queries->next->value));

        return accumulate_union( cumulative, queries->next->next, table );
    }

