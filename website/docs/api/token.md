---
title: Token
teaser: An individual token — i.e. a word, punctuation symbol, whitespace, etc.
tag: class
source: spacy/tokens/token.pyx
---

## Token.\_\_init\_\_ {#init tag="method"}

Construct a `Token` object.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> token = doc[0]
> assert token.text == "Give"
> ```

| Name     | Description                                         |
| -------- | --------------------------------------------------- |
| `vocab`  | A storage container for lexical types. ~~Vocab~~    |
| `doc`    | The parent document. ~~Doc~~                        |
| `offset` | The index of the token within the document. ~~int~~ |

## Token.\_\_len\_\_ {#len tag="method"}

The number of unicode characters in the token, i.e. `token.text`.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> token = doc[0]
> assert len(token) == 4
> ```

| Name        | Description                                            |
| ----------- | ------------------------------------------------------ |
| **RETURNS** | The number of unicode characters in the token. ~~int~~ |

## Token.set_extension {#set_extension tag="classmethod" new="2"}

Define a custom attribute on the `Token` which becomes available via `Token._`.
For details, see the documentation on
[custom attributes](/usage/processing-pipelines#custom-components-attributes).

> #### Example
>
> ```python
> from spacy.tokens import Token
> fruit_getter = lambda token: token.text in ("apple", "pear", "banana")
> Token.set_extension("is_fruit", getter=fruit_getter)
> doc = nlp("I have an apple")
> assert doc[3]._.is_fruit
> ```

| Name      | Description                                                                                                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`    | Name of the attribute to set by the extension. For example, `"my_attr"` will be available as `token._.my_attr`. ~~str~~                                                            |
| `default` | Optional default value of the attribute if no getter or method is defined. ~~Optional[Any]~~                                                                                       |
| `method`  | Set a custom method on the object, for example `token._.compare(other_token)`. ~~Optional[Callable[[Token, ...], Any]]~~                                                           |
| `getter`  | Getter function that takes the object and returns an attribute value. Is called when the user accesses the `._` attribute. ~~Optional[Callable[[Token], Any]]~~                    |
| `setter`  | Setter function that takes the `Token` and a value, and modifies the object. Is called when the user writes to the `Token._` attribute. ~~Optional[Callable[[Token, Any], None]]~~ |
| `force`   | Force overwriting existing attribute. ~~bool~~                                                                                                                                     |

## Token.get_extension {#get_extension tag="classmethod" new="2"}

Look up a previously registered extension by name. Returns a 4-tuple
`(default, method, getter, setter)` if the extension is registered. Raises a
`KeyError` otherwise.

> #### Example
>
> ```python
> from spacy.tokens import Token
> Token.set_extension("is_fruit", default=False)
> extension = Token.get_extension("is_fruit")
> assert extension == (False, None, None, None)
> ```

| Name        | Description                                                                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`      | Name of the extension. ~~str~~                                                                                                                     |
| **RETURNS** | A `(default, method, getter, setter)` tuple of the extension. ~~Tuple[Optional[Any], Optional[Callable], Optional[Callable], Optional[Callable]]~~ |

## Token.has_extension {#has_extension tag="classmethod" new="2"}

Check whether an extension has been registered on the `Token` class.

> #### Example
>
> ```python
> from spacy.tokens import Token
> Token.set_extension("is_fruit", default=False)
> assert Token.has_extension("is_fruit")
> ```

| Name        | Description                                         |
| ----------- | --------------------------------------------------- |
| `name`      | Name of the extension to check. ~~str~~             |
| **RETURNS** | Whether the extension has been registered. ~~bool~~ |

## Token.remove_extension {#remove_extension tag="classmethod" new=""2.0.11""}

Remove a previously registered extension.

> #### Example
>
> ```python
> from spacy.tokens import Token
> Token.set_extension("is_fruit", default=False)
> removed = Token.remove_extension("is_fruit")
> assert not Token.has_extension("is_fruit")
> ```

| Name        | Description                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`      | Name of the extension. ~~str~~                                                                                                                             |
| **RETURNS** | A `(default, method, getter, setter)` tuple of the removed extension. ~~Tuple[Optional[Any], Optional[Callable], Optional[Callable], Optional[Callable]]~~ |

## Token.check_flag {#check_flag tag="method"}

Check the value of a boolean flag.

> #### Example
>
> ```python
> from spacy.attrs import IS_TITLE
> doc = nlp("Give it back! He pleaded.")
> token = doc[0]
> assert token.check_flag(IS_TITLE) == True
> ```

| Name        | Description                                    |
| ----------- | ---------------------------------------------- |
| `flag_id`   | The attribute ID of the flag to check. ~~int~~ |
| **RETURNS** | Whether the flag is set. ~~bool~~              |

## Token.similarity {#similarity tag="method" model="vectors"}

Compute a semantic similarity estimate. Defaults to cosine over vectors.

> #### Example
>
> ```python
> apples, _, oranges = nlp("apples and oranges")
> apples_oranges = apples.similarity(oranges)
> oranges_apples = oranges.similarity(apples)
> assert apples_oranges == oranges_apples
> ```

| Name        | Description                                                                                                                      |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| other       | The object to compare with. By default, accepts `Doc`, `Span`, `Token` and `Lexeme` objects. ~~Union[Doc, Span, Token, Lexeme]~~ |
| **RETURNS** | A scalar similarity score. Higher is more similar. ~~float~~                                                                     |

## Token.nbor {#nbor tag="method"}

Get a neighboring token.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> give_nbor = doc[0].nbor()
> assert give_nbor.text == "it"
> ```

| Name        | Description                                                         |
| ----------- | ------------------------------------------------------------------- |
| `i`         | The relative position of the token to get. Defaults to `1`. ~~int~~ |
| **RETURNS** | The token at position `self.doc[self.i+i]`. ~~Token~~               |

## Token.set_morph {#set_morph tag="method"}

Set the morphological analysis from a UD FEATS string, hash value of a UD FEATS
string, features dict or `MorphAnalysis`. The value `None` can be used to reset
the morph to an unset state.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> doc[0].set_morph("Mood=Imp|VerbForm=Fin")
> assert "Mood=Imp" in doc[0].morph
> assert doc[0].morph.get("Mood") == ["Imp"]
> ```

| Name     | Description                                                                       |
| -------- | --------------------------------------------------------------------------------- |
| features | The morphological features to set. ~~Union[int, dict, str, MorphAnalysis, None]~~ |

## Token.has_morph {#has_morph tag="method"}

Check whether the token has annotated morph information. Return `False` when the
morph annotation is unset/missing.

| Name        | Description                                   |
| ----------- | --------------------------------------------- |
| **RETURNS** | Whether the morph annotation is set. ~~bool~~ |

## Token.is_ancestor {#is_ancestor tag="method" model="parser"}

Check whether this token is a parent, grandparent, etc. of another in the
dependency tree.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> give = doc[0]
> it = doc[1]
> assert give.is_ancestor(it)
> ```

| Name        | Description                                                    |
| ----------- | -------------------------------------------------------------- |
| descendant  | Another token. ~~Token~~                                       |
| **RETURNS** | Whether this token is the ancestor of the descendant. ~~bool~~ |

## Token.ancestors {#ancestors tag="property" model="parser"}

A sequence of the token's syntactic ancestors (parents, grandparents, etc).

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> it_ancestors = doc[1].ancestors
> assert [t.text for t in it_ancestors] == ["Give"]
> he_ancestors = doc[4].ancestors
> assert [t.text for t in he_ancestors] == ["pleaded"]
> ```

| Name       | Description                                                                     |
| ---------- | ------------------------------------------------------------------------------- |
| **YIELDS** | A sequence of ancestor tokens such that `ancestor.is_ancestor(self)`. ~~Token~~ |

## Token.conjuncts {#conjuncts tag="property" model="parser"}

A tuple of coordinated tokens, not including the token itself.

> #### Example
>
> ```python
> doc = nlp("I like apples and oranges")
> apples_conjuncts = doc[2].conjuncts
> assert [t.text for t in apples_conjuncts] == ["oranges"]
> ```

| Name        | Description                                   |
| ----------- | --------------------------------------------- |
| **RETURNS** | The coordinated tokens. ~~Tuple[Token, ...]~~ |

## Token.children {#children tag="property" model="parser"}

A sequence of the token's immediate syntactic children.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> give_children = doc[0].children
> assert [t.text for t in give_children] == ["it", "back", "!"]
> ```

| Name       | Description                                             |
| ---------- | ------------------------------------------------------- |
| **YIELDS** | A child token such that `child.head == self`. ~~Token~~ |

## Token.lefts {#lefts tag="property" model="parser"}

The leftward immediate children of the word in the syntactic dependency parse.

> #### Example
>
> ```python
> doc = nlp("I like New York in Autumn.")
> lefts = [t.text for t in doc[3].lefts]
> assert lefts == ["New"]
> ```

| Name       | Description                          |
| ---------- | ------------------------------------ |
| **YIELDS** | A left-child of the token. ~~Token~~ |

## Token.rights {#rights tag="property" model="parser"}

The rightward immediate children of the word in the syntactic dependency parse.

> #### Example
>
> ```python
> doc = nlp("I like New York in Autumn.")
> rights = [t.text for t in doc[3].rights]
> assert rights == ["in"]
> ```

| Name       | Description                           |
| ---------- | ------------------------------------- |
| **YIELDS** | A right-child of the token. ~~Token~~ |

## Token.n_lefts {#n_lefts tag="property" model="parser"}

The number of leftward immediate children of the word in the syntactic
dependency parse.

> #### Example
>
> ```python
> doc = nlp("I like New York in Autumn.")
> assert doc[3].n_lefts == 1
> ```

| Name        | Description                              |
| ----------- | ---------------------------------------- |
| **RETURNS** | The number of left-child tokens. ~~int~~ |

## Token.n_rights {#n_rights tag="property" model="parser"}

The number of rightward immediate children of the word in the syntactic
dependency parse.

> #### Example
>
> ```python
> doc = nlp("I like New York in Autumn.")
> assert doc[3].n_rights == 1
> ```

| Name        | Description                               |
| ----------- | ----------------------------------------- |
| **RETURNS** | The number of right-child tokens. ~~int~~ |

## Token.subtree {#subtree tag="property" model="parser"}

A sequence containing the token and all the token's syntactic descendants.

> #### Example
>
> ```python
> doc = nlp("Give it back! He pleaded.")
> give_subtree = doc[0].subtree
> assert [t.text for t in give_subtree] == ["Give", "it", "back", "!"]
> ```

| Name       | Description                                                                          |
| ---------- | ------------------------------------------------------------------------------------ |
| **YIELDS** | A descendant token such that `self.is_ancestor(token)` or `token == self`. ~~Token~~ |

## Token.has_vector {#has_vector tag="property" model="vectors"}

A boolean value indicating whether a word vector is associated with the token.

> #### Example
>
> ```python
> doc = nlp("I like apples")
> apples = doc[2]
> assert apples.has_vector
> ```

| Name        | Description                                            |
| ----------- | ------------------------------------------------------ |
| **RETURNS** | Whether the token has a vector data attached. ~~bool~~ |

## Token.vector {#vector tag="property" model="vectors"}

A real-valued meaning representation.

> #### Example
>
> ```python
> doc = nlp("I like apples")
> apples = doc[2]
> assert apples.vector.dtype == "float32"
> assert apples.vector.shape == (300,)
> ```

| Name        | Description                                                                                     |
| ----------- | ----------------------------------------------------------------------------------------------- |
| **RETURNS** | A 1-dimensional array representing the token's vector. ~~numpy.ndarray[ndim=1, dtype=float32]~~ |

## Token.vector_norm {#vector_norm tag="property" model="vectors"}

The L2 norm of the token's vector representation.

> #### Example
>
> ```python
> doc = nlp("I like apples and pasta")
> apples = doc[2]
> pasta = doc[4]
> apples.vector_norm  # 6.89589786529541
> pasta.vector_norm  # 7.759851932525635
> assert apples.vector_norm != pasta.vector_norm
> ```

| Name        | Description                                         |
| ----------- | --------------------------------------------------- |
| **RETURNS** | The L2 norm of the vector representation. ~~float~~ |

## Attributes {#attributes}

| Name                               | Description                                                                                                                                                                                                                                                          |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `doc`                              | The parent document. ~~Doc~~                                                                                                                                                                                                                                         |
| `lex` <Tag variant="new">3</Tag>   | The underlying lexeme. ~~Lexeme~~                                                                                                                                                                                                                                    |
| `sent`                             | The sentence span that this token is a part of. ~~Span~~                                                                                                                                                                                                             |
| `text`                             | Verbatim text content. ~~str~~                                                                                                                                                                                                                                       |
| `text_with_ws`                     | Text content, with trailing space character if present. ~~str~~                                                                                                                                                                                                      |
| `whitespace_`                      | Trailing space character if present. ~~str~~                                                                                                                                                                                                                         |
| `orth`                             | ID of the verbatim text content. ~~int~~                                                                                                                                                                                                                             |
| `orth_`                            | Verbatim text content (identical to `Token.text`). Exists mostly for consistency with the other attributes. ~~str~~                                                                                                                                                  |
| `vocab`                            | The vocab object of the parent `Doc`. ~~vocab~~                                                                                                                                                                                                                      |
| `tensor`                           | The token's slice of the parent `Doc`'s tensor. ~~numpy.ndarray~~                                                                                                                                                                                                    |
| `head`                             | The syntactic parent, or "governor", of this token. ~~Token~~                                                                                                                                                                                                        |
| `left_edge`                        | The leftmost token of this token's syntactic descendants. ~~Token~~                                                                                                                                                                                                  |
| `right_edge`                       | The rightmost token of this token's syntactic descendants. ~~Token~~                                                                                                                                                                                                 |
| `i`                                | The index of the token within the parent document. ~~int~~                                                                                                                                                                                                           |
| `ent_type`                         | Named entity type. ~~int~~                                                                                                                                                                                                                                           |
| `ent_type_`                        | Named entity type. ~~str~~                                                                                                                                                                                                                                           |
| `ent_iob`                          | IOB code of named entity tag. `3` means the token begins an entity, `2` means it is outside an entity, `1` means it is inside an entity, and `0` means no entity tag is set. ~~int~~                                                                                 |
| `ent_iob_`                         | IOB code of named entity tag. "B" means the token begins an entity, "I" means it is inside an entity, "O" means it is outside an entity, and "" means no entity tag is set. ~~str~~                                                                                  |
| `ent_kb_id`                        | Knowledge base ID that refers to the named entity this token is a part of, if any. ~~int~~                                                                                                                                                                           |
| `ent_kb_id_`                       | Knowledge base ID that refers to the named entity this token is a part of, if any. ~~str~~                                                                                                                                                                           |
| `ent_id`                           | ID of the entity the token is an instance of, if any. Currently not used, but potentially for coreference resolution. ~~int~~                                                                                                                                        |
| `ent_id_`                          | ID of the entity the token is an instance of, if any. Currently not used, but potentially for coreference resolution. ~~str~~                                                                                                                                        |
| `lemma`                            | Base form of the token, with no inflectional suffixes. ~~int~~                                                                                                                                                                                                       |
| `lemma_`                           | Base form of the token, with no inflectional suffixes. ~~str~~                                                                                                                                                                                                       |
| `norm`                             | The token's norm, i.e. a normalized form of the token text. Can be set in the language's [tokenizer exceptions](/usage/linguistic-features#language-data). ~~int~~                                                                                                   |
| `norm_`                            | The token's norm, i.e. a normalized form of the token text. Can be set in the language's [tokenizer exceptions](/usage/linguistic-features#language-data). ~~str~~                                                                                                   |
| `lower`                            | Lowercase form of the token. ~~int~~                                                                                                                                                                                                                                 |
| `lower_`                           | Lowercase form of the token text. Equivalent to `Token.text.lower()`. ~~str~~                                                                                                                                                                                        |
| `shape`                            | Transform of the token's string to show orthographic features. Alphabetic characters are replaced by `x` or `X`, and numeric characters are replaced by `d`, and sequences of the same character are truncated after length 4. For example,`"Xxxx"`or`"dd"`. ~~int~~ |
| `shape_`                           | Transform of the token's string to show orthographic features. Alphabetic characters are replaced by `x` or `X`, and numeric characters are replaced by `d`, and sequences of the same character are truncated after length 4. For example,`"Xxxx"`or`"dd"`. ~~str~~ |
| `prefix`                           | Hash value of a length-N substring from the start of the token. Defaults to `N=1`. ~~int~~                                                                                                                                                                           |
| `prefix_`                          | A length-N substring from the start of the token. Defaults to `N=1`. ~~str~~                                                                                                                                                                                         |
| `suffix`                           | Hash value of a length-N substring from the end of the token. Defaults to `N=3`. ~~int~~                                                                                                                                                                             |
| `suffix_`                          | Length-N substring from the end of the token. Defaults to `N=3`. ~~str~~                                                                                                                                                                                             |
| `is_alpha`                         | Does the token consist of alphabetic characters? Equivalent to `token.text.isalpha()`. ~~bool~~                                                                                                                                                                      |
| `is_ascii`                         | Does the token consist of ASCII characters? Equivalent to `all(ord(c) < 128 for c in token.text)`. ~~bool~~                                                                                                                                                          |
| `is_digit`                         | Does the token consist of digits? Equivalent to `token.text.isdigit()`. ~~bool~~                                                                                                                                                                                     |
| `is_lower`                         | Is the token in lowercase? Equivalent to `token.text.islower()`. ~~bool~~                                                                                                                                                                                            |
| `is_upper`                         | Is the token in uppercase? Equivalent to `token.text.isupper()`. ~~bool~~                                                                                                                                                                                            |
| `is_title`                         | Is the token in titlecase? Equivalent to `token.text.istitle()`. ~~bool~~                                                                                                                                                                                            |
| `is_punct`                         | Is the token punctuation? ~~bool~~                                                                                                                                                                                                                                   |
| `is_left_punct`                    | Is the token a left punctuation mark, e.g. `"("` ? ~~bool~~                                                                                                                                                                                                          |
| `is_right_punct`                   | Is the token a right punctuation mark, e.g. `")"` ? ~~bool~~                                                                                                                                                                                                         |
| `is_sent_start`                    | Does the token start a sentence? ~~bool~~ or `None` if unknown. Defaults to `True` for the first token in the `Doc`.                                                                                                                                                 |
| `is_sent_end`                      | Does the token end a sentence? ~~bool~~ or `None` if unknown.                                                                                                                                                                                                        |
| `is_space`                         | Does the token consist of whitespace characters? Equivalent to `token.text.isspace()`. ~~bool~~                                                                                                                                                                      |
| `is_bracket`                       | Is the token a bracket? ~~bool~~                                                                                                                                                                                                                                     |
| `is_quote`                         | Is the token a quotation mark? ~~bool~~                                                                                                                                                                                                                              |
| `is_currency`                      | Is the token a currency symbol? ~~bool~~                                                                                                                                                                                                                             |
| `like_url`                         | Does the token resemble a URL? ~~bool~~                                                                                                                                                                                                                              |
| `like_num`                         | Does the token represent a number? e.g. "10.9", "10", "ten", etc. ~~bool~~                                                                                                                                                                                           |
| `like_email`                       | Does the token resemble an email address? ~~bool~~                                                                                                                                                                                                                   |
| `is_oov`                           | Is the token out-of-vocabulary (i.e. does it not have a word vector)? ~~bool~~                                                                                                                                                                                       |
| `is_stop`                          | Is the token part of a "stop list"? ~~bool~~                                                                                                                                                                                                                         |
| `pos`                              | Coarse-grained part-of-speech from the [Universal POS tag set](https://universaldependencies.org/u/pos/). ~~int~~                                                                                                                                                    |
| `pos_`                             | Coarse-grained part-of-speech from the [Universal POS tag set](https://universaldependencies.org/u/pos/). ~~str~~                                                                                                                                                    |
| `tag`                              | Fine-grained part-of-speech. ~~int~~                                                                                                                                                                                                                                 |
| `tag_`                             | Fine-grained part-of-speech. ~~str~~                                                                                                                                                                                                                                 |
| `morph` <Tag variant="new">3</Tag> | Morphological analysis. ~~MorphAnalysis~~                                                                                                                                                                                                                            |
| `dep`                              | Syntactic dependency relation. ~~int~~                                                                                                                                                                                                                               |
| `dep_`                             | Syntactic dependency relation. ~~str~~                                                                                                                                                                                                                               |
| `lang`                             | Language of the parent document's vocabulary. ~~int~~                                                                                                                                                                                                                |
| `lang_`                            | Language of the parent document's vocabulary. ~~str~~                                                                                                                                                                                                                |
| `prob`                             | Smoothed log probability estimate of token's word type (context-independent entry in the vocabulary). ~~float~~                                                                                                                                                      |
| `idx`                              | The character offset of the token within the parent document. ~~int~~                                                                                                                                                                                                |
| `sentiment`                        | A scalar value indicating the positivity or negativity of the token. ~~float~~                                                                                                                                                                                       |
| `lex_id`                           | Sequential ID of the token's lexical type, used to index into tables, e.g. for word vectors. ~~int~~                                                                                                                                                                 |
| `rank`                             | Sequential ID of the token's lexical type, used to index into tables, e.g. for word vectors. ~~int~~                                                                                                                                                                 |
| `cluster`                          | Brown cluster ID. ~~int~~                                                                                                                                                                                                                                            |
| `_`                                | User space for adding custom [attribute extensions](/usage/processing-pipelines#custom-components-attributes). ~~Underscore~~                                                                                                                                        |
