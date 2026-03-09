file: 

apple
orange
banana
pappaya

```bash
$ sed 's/.*/& is a Fruit/' sample1.txt
```

'.*' matches line till line break

The '&' symbol denotes the entire pattern which is  matched.

after executing above command.

apple is a Fruit
orange is a Fruit
banana is a Fruit
pappaya is a Fruit