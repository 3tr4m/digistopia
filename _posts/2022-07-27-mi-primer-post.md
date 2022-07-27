---
# layout: post
title: "Revisión de textos con RegEx"
categories:
  - RegEx
tags:
  - Revisión de textos
  - RegEx
last_modified_at: 2022-07-27T14:25:52-05:00
---


# Cómo usar Expresiones Regulares para revisar y reemplazar errores comunes en tus documentos

Esta pretende ser una guía rápida para aquellos que continuamente hacemos trabajo de escritura , especialmente en el desarrollo de trabajos extensos. La idea es proveer un set de fórmulas de expresiones regulares que puedan ser usadas en la revisión de textos. Dado que mis principales actividades de escritura provienen de la Universidad y la Investigación, la mayor parte de este set será especialmente útil para los académicos. 

Puede que muchos no lo sepan, pero por lo general los procesadores de texto ofrecen la posibilidad de buscar patrones en los textos de forma que sea más fácil hacer cambios de forma rápida y masiva. Esto, por supuesto, resulta muy práctico en textos extensos.

---

<!—Hay que señalar los que son manuales y los que admiten reemplazo automático —



- Buscar palabras repetidas

    ```regex
    \b(\w+\b *)\b\1{1,}
    ```


- Buscar puntuación dentro y fuera de final de cita

    ```regexp
    [.,;:]”
    ```
    
    ```.regexp
    ”[.,;:]
    ```

- Convertir tres puntos seguidos “`...`” en el caracter de puntos suspensivos “`…`”
Búsqueda: 

```.regex
[.]{3}
```
Reemplazo: 
```regexp
…
```

- Buscar cualquier caracter repetido, siempre que no sea una letra, número o espacio.
Ahora que tenemos los tres puntos, podemos buscar cualquier caracter repetido sin preocuparnos por modificar los tres puntos, que son gramaticalmente aceptables. Ejemplos: `?? // ;; ,, ,,, `

Búsqueda: 
```regexp
([^\w\d\s])\1{1,}`
```
Reemplazo: 
```regexp
$1
```

- Caracteres diferentes consecutivos 
Después de haber reemplazado aquellos signos duplicados, podemos buscar aquellos signos consecutivos que no son una repetición, pero que pueden ser repeticiones.

Búsqueda:

```regexp
([^\w\d\s]){2,}`
```

Ejemplos: `., ;: ,. ?!` 

- Eliminar espacio(s) a inicio y final de párrafo
Búsqueda:
```regexp
^ +| +$()`
```
Reemplazo: 
```regexp
$1`
```

- Símbolos abiertos, pero no cerrados `‘ “ ( [ ¿ ¡`

```regexp
[“][^”\n]*\n|[(][^)\n]*\n|[[][^\]\n]*\n|[‘][^’\n]*\n|[¿][^?\n]*\n|[¡][^!\n]*\n`
```

- Símbolos cerrados, pero no abiertos `’ ” ) ] ? !`

```regexp
^[^\n“]*”|^[^\n‘]*’|^[^\n(]*\)|^[^\n[]*]|^[^\n¿]*?|^[^\n¡]*!`
```

- Párrafos que no terminan en puntuación admitida
Búsqueda: 
```regexp
[^.:?!\]]$`
```

- Párrafos que no terminan en puntuación admitida
Búsqueda: 
```regexp
[^.:?!\]]$`
```

- Falsas comillas `' "` para reemplazar por las a propiadas `“”` o `‘’`
Búsqueda: 
```regexp
['"]`
```



==Falta hacer la guía para puntuación antes y después de conjunciones y conectores gramaticales==

Comma after a conjunction:  
Regex: b(but|and|so|which|yet|or|except),  
Sigil:  b(but|and|so|which|yet|or|except),  
Word: ”

==Buscar referencias internas únicas, es decir los `[@sec:]` que no están vinculados a un `{#sec:)`==

`\b(\w+\b *)\b\1{1,}`

Para buscar las referencias pandoc [@key] seguidas de puntuación e inveertirlas.
```regexp

[\.?](?=[ [])(?! *[\n\da-z])

```

==Buscar espacios antes de puntuación==

- Reemplazar luego de punto con saltos de línea (En mi tesis)
Sirve para adoptar el estilo de esccritura de una línea por oración el cual no afecta el output de pandoc. (Los inicios de cada línea no necesitan tener espacio, Pandoc lo corrige solito)

```regexp
[\.?](?=[ [])(?! *[\n\da-z])`
```

