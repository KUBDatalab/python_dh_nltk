---
title: "Python, Digital Humaniora og NLTK"
teaching: 10
exercises: 0
questions:
- "Dyk ned i NTLK"
objectives:
- "Loops"
- "Lists"
- "Stings"

keypoints:
- "Kendskab til NLTK"
---

# HER STARTER VI !!

```python
import urllib.request
import re
import nltk
```


```python
# hent teksten
url = 'https://www.gutenberg.org/files/2591/2591-0.txt'
raw_text = urllib.request.urlopen(url).read().decode()
```


```python
#find starten
start = raw_text.find('THE BROTHERS GRIMM FAIRY TALES') + len('THE BROTHERS GRIMM FAIRY TALES')
# find slutningen
end = raw_text.find('*****')
# find det der imellem
content = raw_text[start:end]

#opdel på hvert eventyr 
tales = content.split('\r\n\r\n\r\n\r\n\r\n') # split før overskrift på titel
# Filtrer første item væk, fordi det er tomt
tales = tales[1:] 

titles = []
for i in tales:
    split_text = i.split('\r\n\r\n\r\n') # split efter titel
    title = split_text[0] # gem første del af splittet - det er titlen  
    titles.append(title) # tilføj titlen til den tomme liste

# zip titles og tales    
zip_format = list(zip(titles, tales))
zip_format
```


```Out




    [('THE GOLDEN BIRD',
      'THE GOLDEN BIRD\r\n\r\n\r\nA certain king had a beautiful garden, and in the garden stood a tree\r\nwhich bore golden apples. These apples were always counted, and about\r\nthe time when they began to grow ripe it was found that every night one\r\nof them was gone. The king became very angry at this, and ordered the\r\ngardener to keep watch all night under the tree. The gardener set his\r\neldest son to watch; but about twelve o’clock he fell asleep, and in\r\nthe morning another of the apples was missing. Then the second son was\r\nordered to watch; and at midnight he too fell asleep, and in the morning\r\nanother apple was gone. Then the third son offered to keep watch; but\r\nthe gardener at first would not let him, for fear some harm should come\r\nto him: however, at last he consented, and the young man laid himself\r\nunder the tree to watch. As the clock struck twelve he heard a rustling\r\nnoise in the air, and a bird came flying that was of pure gold; and as\r\nit was snapping at one of the apples with its beak, the gardener’s son\r\njumped up and shot an arrow at it. But the arrow did the bird no harm;\r\nonly it dropped a golden feather from its tail, and then flew away.\r\nThe golden feather was brought to the king in the morning, and all the\r\ncouncil was called together. Everyone agreed that it was worth more than\r\nall the wealth of the kingdom: but the king said, ‘One feather is of no\r\nuse to me, I must have the whole bird.’\r\n\r\nThen the gardener’s eldest son set out and thought to find the golden\r\nbird very easily; and when he had gone but a little way, he came to a\r\nwood, and by the side of the wood he saw a fox sitting; so he took his\r\nbow and made ready to shoot at it. Then the fox said, ‘Do not shoot me,\r\nfor I will give you good counsel; I know what your business is, and\r\nthat you want to find the golden bird. You will reach a village in the\r\nevening; and when you get there, you will see two inns opposite to each\r\nother, one of which is very pleasant and beautiful to look at: go not in\r\nthere, but rest for the night in the other, though it may appear to you\r\nto be very poor and mean.’ But the son thought to himself, ‘What can\r\nsuch a beast as this know about the matter?’ So he shot his arrow at\r\nthe fox; but he missed it, and it set up its tail above its back and\r\nran into the wood. Then he went his way, and in the evening came to\r\nthe village where the two inns were; and in one of these were people\r\nsinging, and dancing, and feasting; but the other looked very dirty,\r\nand poor. ‘I should be very silly,’ said he, ‘if I went to that shabby\r\nhouse, and left this charming place’; so he went into the smart house,\r\nand ate and drank at his ease, and forgot the bird, and his country too.\r\n\r\nTime passed on; and as the eldest son did not come back, and no tidings\r\nwere heard of him, the second son set out, and the same thing happened\r\nto him. He met the fox, who gave him the good advice: but when he came\r\nto the two inns, his eldest brother was standing at the window where\r\nthe merrymaking was, and called to him to come in; and he could not\r\nwithstand the temptation, but went in, and forgot the golden bird and\r\nhis country in the same manner.\r\n\r\nTime passed on again, and the youngest son too wished to set out into\r\nthe wide world to seek for the golden bird; but his father would not\r\nlisten to it for a long while, for he was very fond of his son, and\r\nwas afraid that some ill luck might happen to him also, and prevent his\r\ncoming back. However, at last it was agreed he should go, for he would\r\nnot rest at home; and as he came to the wood, he met the fox, and heard\r\nthe same good counsel. But he was thankful to the fox, and did not\r\nattempt his life as his brothers had done; so the fox said, ‘Sit upon my\r\ntail, and you will travel faster.’ So he sat down, and the fox began to\r\nrun, and away they went over stock and stone so quick that their hair\r\nwhistled in the wind.\r\n\r\nWhen they came to the village, the son followed the fox’s counsel, and\r\nwithout looking about him went to the shabby inn and rested there all\r\nnight at his ease. In the morning came the fox again and met him as he\r\nwas beginning his journey, and said, ‘Go straight forward, till you come\r\nto a castle, before which lie a whole troop of soldiers fast asleep and\r\nsnoring: take no notice of them, but go into the castle and pass on and\r\non till you come to a room, where the golden bird sits in a wooden cage;\r\nclose by it stands a beautiful golden cage; but do not try to take the\r\nbird out of the shabby cage and put it into the handsome one, otherwise\r\nyou will repent it.’ Then the fox stretched out his tail again, and the\r\nyoung man sat himself down, and away they went over stock and stone till\r\ntheir hair whistled in the wind.\r\n\r\nBefore the castle gate all was as the fox had said: so the son went in\r\nand found the chamber where the golden bird hung in a wooden cage, and\r\nbelow stood the golden cage, and the three golden apples that had been\r\nlost were lying close by it. Then thought he to himself, ‘It will be a\r\nvery droll thing to bring away such a fine bird in this shabby cage’; so\r\nhe opened the door and took hold of it and put it into the golden cage.\r\nBut the bird set up such a loud scream that all the soldiers awoke, and\r\nthey took him prisoner and carried him before the king. The next morning\r\nthe court sat to judge him; and when all was heard, it sentenced him to\r\ndie, unless he should bring the king the golden horse which could run as\r\nswiftly as the wind; and if he did this, he was to have the golden bird\r\ngiven him for his own.\r\n\r\nSo he set out once more on his journey, sighing, and in great despair,\r\nwhen on a sudden his friend the fox met him, and said, ‘You see now\r\nwhat has happened on account of your not listening to my counsel. I will\r\nstill, however, tell you how to find the golden horse, if you will do as\r\nI bid you. You must go straight on till you come to the castle where the\r\nhorse stands in his stall: by his side will lie the groom fast asleep\r\nand snoring: take away the horse quietly, but be sure to put the old\r\nleathern saddle upon him, and not the golden one that is close by it.’\r\nThen the son sat down on the fox’s tail, and away they went over stock\r\nand stone till their hair whistled in the wind.\r\n\r\nAll went right, and the groom lay snoring with his hand upon the golden\r\nsaddle. But when the son looked at the horse, he thought it a great pity\r\nto put the leathern saddle upon it. ‘I will give him the good one,’\r\nsaid he; ‘I am sure he deserves it.’ As he took up the golden saddle the\r\ngroom awoke and cried out so loud, that all the guards ran in and took\r\nhim prisoner, and in the morning he was again brought before the court\r\nto be judged, and was sentenced to die. But it was agreed, that, if he\r\ncould bring thither the beautiful princess, he should live, and have the\r\nbird and the horse given him for his own.\r\n\r\nThen he went his way very sorrowful; but the old fox came and said, ‘Why\r\ndid not you listen to me? If you had, you would have carried away\r\nboth the bird and the horse; yet will I once more give you counsel. Go\r\nstraight on, and in the evening you will arrive at a castle. At twelve\r\no’clock at night the princess goes to the bathing-house: go up to her\r\nand give her a kiss, and she will let you lead her away; but take care\r\nyou do not suffer her to go and take leave of her father and mother.’\r\nThen the fox stretched out his tail, and so away they went over stock\r\nand stone till their hair whistled again.\r\n\r\nAs they came to the castle, all was as the fox had said, and at twelve\r\no’clock the young man met the princess going to the bath and gave her the\r\nkiss, and she agreed to run away with him, but begged with many tears\r\nthat he would let her take leave of her father. At first he refused,\r\nbut she wept still more and more, and fell at his feet, till at last\r\nhe consented; but the moment she came to her father’s house the guards\r\nawoke and he was taken prisoner again.\r\n\r\nThen he was brought before the king, and the king said, ‘You shall never\r\nhave my daughter unless in eight days you dig away the hill that stops\r\nthe view from my window.’ Now this hill was so big that the whole world\r\ncould not take it away: and when he had worked for seven days, and had\r\ndone very little, the fox came and said. ‘Lie down and go to sleep; I\r\nwill work for you.’ And in the morning he awoke and the hill was gone;\r\nso he went merrily to the king, and told him that now that it was\r\nremoved he must give him the princess.\r\n\r\nThen the king was obliged to keep his word, and away went the young man\r\nand the princess; and the fox came and said to him, ‘We will have all\r\nthree, the princess, the horse, and the bird.’ ‘Ah!’ said the young man,\r\n‘that would be a great thing, but how can you contrive it?’\r\n\r\n‘If you will only listen,’ said the fox, ‘it can be done. When you come\r\nto the king, and he asks for the beautiful princess, you must say, “Here\r\nshe is!” Then he will be very joyful; and you will mount the golden\r\nhorse that they are to give you, and put out your hand to take leave of\r\nthem; but shake hands with the princess last. Then lift her quickly on\r\nto the horse behind you; clap your spurs to his side, and gallop away as\r\nfast as you can.’\r\n\r\nAll went right: then the fox said, ‘When you come to the castle where\r\nthe bird is, I will stay with the princess at the door, and you will\r\nride in and speak to the king; and when he sees that it is the right\r\nhorse, he will bring out the bird; but you must sit still, and say that\r\nyou want to look at it, to see whether it is the true golden bird; and\r\nwhen you get it into your hand, ride away.’\r\n\r\nThis, too, happened as the fox said; they carried off the bird, the\r\nprincess mounted again, and they rode on to a great wood. Then the fox\r\ncame, and said, ‘Pray kill me, and cut off my head and my feet.’ But the\r\nyoung man refused to do it: so the fox said, ‘I will at any rate give\r\nyou good counsel: beware of two things; ransom no one from the gallows,\r\nand sit down by the side of no river.’ Then away he went. ‘Well,’\r\nthought the young man, ‘it is no hard matter to keep that advice.’\r\n\r\nHe rode on with the princess, till at last he came to the village where\r\nhe had left his two brothers. And there he heard a great noise and\r\nuproar; and when he asked what was the matter, the people said, ‘Two men\r\nare going to be hanged.’ As he came nearer, he saw that the two men were\r\nhis brothers, who had turned robbers; so he said, ‘Cannot they in any\r\nway be saved?’ But the people said ‘No,’ unless he would bestow all his\r\nmoney upon the rascals and buy their liberty. Then he did not stay to\r\nthink about the matter, but paid what was asked, and his brothers were\r\ngiven up, and went on with him towards their home.\r\n\r\nAnd as they came to the wood where the fox first met them, it was so\r\ncool and pleasant that the two brothers said, ‘Let us sit down by the\r\nside of the river, and rest a while, to eat and drink.’ So he said,\r\n‘Yes,’ and forgot the fox’s counsel, and sat down on the side of the\r\nriver; and while he suspected nothing, they came behind, and threw him\r\ndown the bank, and took the princess, the horse, and the bird, and went\r\nhome to the king their master, and said. ‘All this have we won by our\r\nlabour.’ Then there was great rejoicing made; but the horse would not\r\neat, the bird would not sing, and the princess wept.\r\n\r\nThe youngest son fell to the bottom of the river’s bed: luckily it was\r\nnearly dry, but his bones were almost broken, and the bank was so steep\r\nthat he could find no way to get out. Then the old fox came once more,\r\nand scolded him for not following his advice; otherwise no evil would\r\nhave befallen him: ‘Yet,’ said he, ‘I cannot leave you here, so lay hold\r\nof my tail and hold fast.’ Then he pulled him out of the river, and said\r\nto him, as he got upon the bank, ‘Your brothers have set watch to kill\r\nyou, if they find you in the kingdom.’ So he dressed himself as a poor\r\nman, and came secretly to the king’s court, and was scarcely within the\r\ndoors when the horse began to eat, and the bird to sing, and the princess\r\nleft off weeping. Then he went to the king, and told him all his\r\nbrothers’ roguery; and they were seized and punished, and he had the\r\nprincess given to him again; and after the king’s death he was heir to\r\nhis kingdom.\r\n\r\nA long while after, he went to walk one day in the wood, and the old fox\r\nmet him, and besought him with tears in his eyes to kill him, and cut\r\noff his head and feet. And at last he did so, and in a moment the\r\nfox was changed into a man, and turned out to be the brother of the\r\nprincess, who had been lost a great many many years.'),
     ('HANS IN LUCK',
      'HANS IN LUCK\r\n\r\n\r\nSome men are born to good luck: all they do or try to do comes\r\nright--all that falls to them is so much gain--all their geese are\r\nswans--all their cards are trumps--toss them which way you will, they\r\nwill always, like poor puss, alight upon their legs, and only move on so\r\nmuch the faster. The world may very likely not always think of them as\r\nthey think of themselves, but what care they for the world? what can it\r\nknow about the matter?\r\n\r\nOne of these lucky beings was neighbour Hans. Seven long years he had\r\nworked hard for his master. At last he said, ‘Master, my time is up; I\r\nmust go home and see my poor mother once more: so pray pay me my wages\r\nand let me go.’ And the master said, ‘You have been a faithful and good\r\nservant, Hans, so your pay shall be handsome.’ Then he gave him a lump\r\nof silver as big as his head.\r\n\r\nHans took out his pocket-handkerchief, put the piece of silver into it,\r\nthrew it over his shoulder, and jogged off on his road homewards. As he\r\nwent lazily on, dragging one foot after another, a man came in sight,\r\ntrotting gaily along on a capital horse. ‘Ah!’ said Hans aloud, ‘what a\r\nfine thing it is to ride on horseback! There he sits as easy and happy\r\nas if he was at home, in the chair by his fireside; he trips against no\r\nstones, saves shoe-leather, and gets on he hardly knows how.’ Hans did\r\nnot speak so softly but the horseman heard it all, and said, ‘Well,\r\nfriend, why do you go on foot then?’ ‘Ah!’ said he, ‘I have this load to\r\ncarry: to be sure it is silver, but it is so heavy that I can’t hold up\r\nmy head, and you must know it hurts my shoulder sadly.’ ‘What do you say\r\nof making an exchange?’ said the horseman. ‘I will give you my horse,\r\nand you shall give me the silver; which will save you a great deal of\r\ntrouble in carrying such a heavy load about with you.’ ‘With all my\r\nheart,’ said Hans: ‘but as you are so kind to me, I must tell you one\r\nthing--you will have a weary task to draw that silver about with you.’\r\nHowever, the horseman got off, took the silver, helped Hans up, gave him\r\nthe bridle into one hand and the whip into the other, and said, ‘When\r\nyou want to go very fast, smack your lips loudly together, and cry\r\n“Jip!”’\r\n\r\nHans was delighted as he sat on the horse, drew himself up, squared his\r\nelbows, turned out his toes, cracked his whip, and rode merrily off, one\r\nminute whistling a merry tune, and another singing,\r\n\r\n ‘No care and no sorrow,\r\n  A fig for the morrow!\r\n  We’ll laugh and be merry,\r\n  Sing neigh down derry!’\r\n\r\nAfter a time he thought he should like to go a little faster, so he\r\nsmacked his lips and cried ‘Jip!’ Away went the horse full gallop; and\r\nbefore Hans knew what he was about, he was thrown off, and lay on his\r\nback by the road-side. His horse would have ran off, if a shepherd who\r\nwas coming by, driving a cow, had not stopped it. Hans soon came to\r\nhimself, and got upon his legs again, sadly vexed, and said to the\r\nshepherd, ‘This riding is no joke, when a man has the luck to get upon\r\na beast like this that stumbles and flings him off as if it would break\r\nhis neck. However, I’m off now once for all: I like your cow now a great\r\ndeal better than this smart beast that played me this trick, and has\r\nspoiled my best coat, you see, in this puddle; which, by the by, smells\r\nnot very like a nosegay. One can walk along at one’s leisure behind that\r\ncow--keep good company, and have milk, butter, and cheese, every day,\r\ninto the bargain. What would I give to have such a prize!’ ‘Well,’ said\r\nthe shepherd, ‘if you are so fond of her, I will change my cow for your\r\nhorse; I like to do good to my neighbours, even though I lose by it\r\nmyself.’ ‘Done!’ said Hans, merrily. ‘What a noble heart that good man\r\nhas!’ thought he. Then the shepherd jumped upon the horse, wished Hans\r\nand the cow good morning, and away he rode.\r\n\r\nHans brushed his coat, wiped his face and hands, rested a while, and\r\nthen drove off his cow quietly, and thought his bargain a very lucky\r\none. ‘If I have only a piece of bread (and I certainly shall always be\r\nable to get that), I can, whenever I like, eat my butter and cheese with\r\nit; and when I am thirsty I can milk my cow and drink the milk: and what\r\ncan I wish for more?’ When he came to an inn, he halted, ate up all his\r\nbread, and gave away his last penny for a glass of beer. When he had\r\nrested himself he set off again, driving his cow towards his mother’s\r\nvillage. But the heat grew greater as soon as noon came on, till at\r\nlast, as he found himself on a wide heath that would take him more than\r\nan hour to cross, he began to be so hot and parched that his tongue\r\nclave to the roof of his mouth. ‘I can find a cure for this,’ thought\r\nhe; ‘now I will milk my cow and quench my thirst’: so he tied her to the\r\nstump of a tree, and held his leathern cap to milk into; but not a drop\r\nwas to be had. Who would have thought that this cow, which was to bring\r\nhim milk and butter and cheese, was all that time utterly dry? Hans had\r\nnot thought of looking to that.\r\n\r\nWhile he was trying his luck in milking, and managing the matter very\r\nclumsily, the uneasy beast began to think him very troublesome; and at\r\nlast gave him such a kick on the head as knocked him down; and there he\r\nlay a long while senseless. Luckily a butcher soon came by, driving a\r\npig in a wheelbarrow. ‘What is the matter with you, my man?’ said the\r\nbutcher, as he helped him up. Hans told him what had happened, how he\r\nwas dry, and wanted to milk his cow, but found the cow was dry too. Then\r\nthe butcher gave him a flask of ale, saying, ‘There, drink and refresh\r\nyourself; your cow will give you no milk: don’t you see she is an old\r\nbeast, good for nothing but the slaughter-house?’ ‘Alas, alas!’ said\r\nHans, ‘who would have thought it? What a shame to take my horse, and\r\ngive me only a dry cow! If I kill her, what will she be good for? I hate\r\ncow-beef; it is not tender enough for me. If it were a pig now--like\r\nthat fat gentleman you are driving along at his ease--one could do\r\nsomething with it; it would at any rate make sausages.’ ‘Well,’ said\r\nthe butcher, ‘I don’t like to say no, when one is asked to do a kind,\r\nneighbourly thing. To please you I will change, and give you my fine fat\r\npig for the cow.’ ‘Heaven reward you for your kindness and self-denial!’\r\nsaid Hans, as he gave the butcher the cow; and taking the pig off the\r\nwheel-barrow, drove it away, holding it by the string that was tied to\r\nits leg.\r\n\r\nSo on he jogged, and all seemed now to go right with him: he had met\r\nwith some misfortunes, to be sure; but he was now well repaid for all.\r\nHow could it be otherwise with such a travelling companion as he had at\r\nlast got?\r\n\r\nThe next man he met was a countryman carrying a fine white goose. The\r\ncountryman stopped to ask what was o’clock; this led to further chat;\r\nand Hans told him all his luck, how he had so many good bargains, and\r\nhow all the world went gay and smiling with him. The countryman then\r\nbegan to tell his tale, and said he was going to take the goose to a\r\nchristening. ‘Feel,’ said he, ‘how heavy it is, and yet it is only eight\r\nweeks old. Whoever roasts and eats it will find plenty of fat upon it,\r\nit has lived so well!’ ‘You’re right,’ said Hans, as he weighed it in\r\nhis hand; ‘but if you talk of fat, my pig is no trifle.’ Meantime the\r\ncountryman began to look grave, and shook his head. ‘Hark ye!’ said he,\r\n‘my worthy friend, you seem a good sort of fellow, so I can’t help doing\r\nyou a kind turn. Your pig may get you into a scrape. In the village I\r\njust came from, the squire has had a pig stolen out of his sty. I was\r\ndreadfully afraid when I saw you that you had got the squire’s pig. If\r\nyou have, and they catch you, it will be a bad job for you. The least\r\nthey will do will be to throw you into the horse-pond. Can you swim?’\r\n\r\nPoor Hans was sadly frightened. ‘Good man,’ cried he, ‘pray get me out\r\nof this scrape. I know nothing of where the pig was either bred or born;\r\nbut he may have been the squire’s for aught I can tell: you know this\r\ncountry better than I do, take my pig and give me the goose.’ ‘I ought\r\nto have something into the bargain,’ said the countryman; ‘give a fat\r\ngoose for a pig, indeed! ‘Tis not everyone would do so much for you as\r\nthat. However, I will not be hard upon you, as you are in trouble.’ Then\r\nhe took the string in his hand, and drove off the pig by a side path;\r\nwhile Hans went on the way homewards free from care. ‘After all,’\r\nthought he, ‘that chap is pretty well taken in. I don’t care whose pig\r\nit is, but wherever it came from it has been a very good friend to me. I\r\nhave much the best of the bargain. First there will be a capital roast;\r\nthen the fat will find me in goose-grease for six months; and then there\r\nare all the beautiful white feathers. I will put them into my pillow,\r\nand then I am sure I shall sleep soundly without rocking. How happy my\r\nmother will be! Talk of a pig, indeed! Give me a fine fat goose.’\r\n\r\nAs he came to the next village, he saw a scissor-grinder with his wheel,\r\nworking and singing,\r\n\r\n ‘O’er hill and o’er dale\r\n  So happy I roam,\r\n  Work light and live well,\r\n  All the world is my home;\r\n  Then who so blythe, so merry as I?’\r\n\r\nHans stood looking on for a while, and at last said, ‘You must be well\r\noff, master grinder! you seem so happy at your work.’ ‘Yes,’ said the\r\nother, ‘mine is a golden trade; a good grinder never puts his hand\r\ninto his pocket without finding money in it--but where did you get that\r\nbeautiful goose?’ ‘I did not buy it, I gave a pig for it.’ ‘And where\r\ndid you get the pig?’ ‘I gave a cow for it.’ ‘And the cow?’ ‘I gave a\r\nhorse for it.’ ‘And the horse?’ ‘I gave a lump of silver as big as my\r\nhead for it.’ ‘And the silver?’ ‘Oh! I worked hard for that seven long\r\nyears.’ ‘You have thriven well in the world hitherto,’ said the grinder,\r\n‘now if you could find money in your pocket whenever you put your hand\r\nin it, your fortune would be made.’ ‘Very true: but how is that to be\r\nmanaged?’ ‘How? Why, you must turn grinder like myself,’ said the other;\r\n‘you only want a grindstone; the rest will come of itself. Here is one\r\nthat is but little the worse for wear: I would not ask more than the\r\nvalue of your goose for it--will you buy?’ ‘How can you ask?’ said\r\nHans; ‘I should be the happiest man in the world, if I could have money\r\nwhenever I put my hand in my pocket: what could I want more? there’s\r\nthe goose.’ ‘Now,’ said the grinder, as he gave him a common rough stone\r\nthat lay by his side, ‘this is a most capital stone; do but work it well\r\nenough, and you can make an old nail cut with it.’\r\n\r\nHans took the stone, and went his way with a light heart: his eyes\r\nsparkled for joy, and he said to himself, ‘Surely I must have been born\r\nin a lucky hour; everything I could want or wish for comes of itself.\r\nPeople are so kind; they seem really to think I do them a favour in\r\nletting them make me rich, and giving me good bargains.’\r\n\r\nMeantime he began to be tired, and hungry too, for he had given away his\r\nlast penny in his joy at getting the cow.\r\n\r\nAt last he could go no farther, for the stone tired him sadly: and he\r\ndragged himself to the side of a river, that he might take a drink of\r\nwater, and rest a while. So he laid the stone carefully by his side on\r\nthe bank: but, as he stooped down to drink, he forgot it, pushed it a\r\nlittle, and down it rolled, plump into the stream.\r\n\r\nFor a while he watched it sinking in the deep clear water; then sprang\r\nup and danced for joy, and again fell upon his knees and thanked Heaven,\r\nwith tears in his eyes, for its kindness in taking away his only plague,\r\nthe ugly heavy stone.\r\n\r\n‘How happy am I!’ cried he; ‘nobody was ever so lucky as I.’ Then up he\r\ngot with a light heart, free from all his troubles, and walked on till\r\nhe reached his mother’s house, and told her how very easy the road to\r\ngood luck was.')]

```

# Os biblioteket


```python
import os
```


```python
os.getcwd()
```

```Out


    'your own path'

```

```python
# lav en ny mappe
# os.mkdir('grimm_tales')
os.chdir('grimm_tales')
```


```python
# Gem filerne i mappen
for i in zip_format:
    new_file = open(i[0]+'.txt', 'w', encoding='utf-8-sig')
    new_file.write(i[1])
    new_file.close()

# check om de er der der!    
os.listdir()
```

```Out


    ['.ipynb_checkpoints',
     'ASHPUTTEL.txt',
     'BRIAR ROSE.txt',
     'CAT AND MOUSE IN PARTNERSHIP.txt',
     'CAT-SKIN.txt',
     'CLEVER ELSIE.txt',
     'CLEVER GRETEL.txt'
	 
	 '...']

````

# NLTK metoder


```python
import nltk
text = tales[0]
lower_text = text.lower()
scrub_text = ' '.join(re.findall(r'\S+', lower_text))
tokenized_text = nltk.word_tokenize(scrub_text)
nltk_text = nltk.Text(tokenized_text)
stopwords = nltk.corpus.stopwords.words('english')
newStopWords = [',',';', '.', ':', '‘', '’']
stopwords.extend(newStopWords)
filtered_tokens = [word for word in nltk_text if word not in stopwords]
filtered_tokens
```
```Out



    ['golden',
     'bird',
     'certain',
     'king',
     'beautiful',
     'garden',
     'garden'
     ...]
```



```python
fdist_filtered = nltk.FreqDist(filtered_tokens).plot(20, title='Hyppigste ord (uden stopord)')
```


    
![png](fig/output_13_0.png)
    


Collocation_list() returnerer en liste over de mest almindelige ordpar i teksten. Bemærk, at i nogle versioner af Python virker collocation_list() ikke. Hvis dette er tilfældet, prøv collocations() i stedet.


```python
nltk_text.collocation_list()
```

```Out

    [('young', 'man'),
     ('golden', 'bird'),
     ('hair', 'whistled'),
     ('two', 'inns'),
     ('stone', 'till'),
     ('take', 'leave'),
     ('good', 'counsel'),
     ('time', 'passed'),
     ('eldest', 'son'),
     ('leathern', 'saddle'),
     ('fox', 'came'),
     ('morning', 'another'),
     ('wooden', 'cage'),
     ('two', 'men'),
     ('fast', 'asleep'),
     ('old', 'fox'),
     ('fell', 'asleep'),
     ('fox', 'said'),
     ('second', 'son'),
     ('youngest', 'son')]

```

Concordance()-metoden returnerer konteksten af et specifikt udtryk. Længden af output kan ændres med parametrene i width og lines.


```python
nltk_text.concordance('golden', lines=30, width=80)
```

    Displaying 22 of 22 matches:
    the golden bird a certain king had a beautiful 
    n the garden stood a tree which bore golden apples . these apples were always co
    the bird no harm ; only it dropped a golden feather from its tail , and then fle
     its tail , and then flew away . the golden feather was brought to the king in t
     son set out and thought to find the golden bird very easily ; and when he had g
    s is , and that you want to find the golden bird . you will reach a village in t
    ation , but went in , and forgot the golden bird and his country in the same man
     into the wide world to seek for the golden bird ; but his father would not list
     till you come to a room , where the golden bird sits in a wooden cage ; close b
    age ; close by it stands a beautiful golden cage ; but do not try to take the bi
    t in and found the chamber where the golden bird hung in a wooden cage , and bel
     a wooden cage , and below stood the golden cage , and the three golden apples t
    tood the golden cage , and the three golden apples that had been lost were lying
     took hold of it and put it into the golden cage . but the bird set up such a lo
     unless he should bring the king the golden horse which could run as swiftly as 
     if he did this , he was to have the golden bird given him for his own . so he s
    , however , tell you how to find the golden horse , if you will do as i bid you 
    athern saddle upon him , and not the golden one that is close by it. ’ then the 
    m lay snoring with his hand upon the golden saddle . but when the son looked at 
     he deserves it. ’ as he took up the golden saddle the groom awoke and cried out
    very joyful ; and you will mount the golden horse that they are to give you , an
    t it , to see whether it is the true golden bird ; and when you get it into your
    

Similar() anvendes til at identificere ord, der optræder i en lignende kontekst.


```python
nltk_text.similar('golden')
```

    good shabby handsome leathern right
    

Dispersion_plot() anvendes til at visualisere, hvordan termer forekommer på tværs af vores tekst. Metoden accepterer en liste med et eller flere udtryk som input.


```python
terms = ['horse', 'bird', 'apples', 'cage']

nltk_text.dispersion_plot(terms)
```


    
![png](fig/output_21_0.png)
    


Generate() anvendes til at genere mere eller mindre sammenhængende tekst med udgangspunkt i en eksisterende tekst.


```python
text_gen = nltk_text.generate(150)
```

    out , and forgot the golden saddle . , that , if you will reach a
    village in the wind . fox met him , and all the council was called
    together . apples was missing . very fond of his son , and went home
    to the village where he had the princess wept . castle and pass on and
    on till you come to a wood , and was sentenced to die . it . . his
    country too . and put it into the smart house , and the king said , ‘
    your brothers have set watch to kill you , if they find you in the
    same manner . ‘ all this have we won by our labour. of two things ;
    ransom no one from the gallows , and the fox said , ‘ one feather is
    of no river. went his way ,
    

    Building ngram index...
    


```python

```



{% include links.md %}

