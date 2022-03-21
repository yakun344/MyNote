# Data Structure

## 1. Hash Collision(Q & A)

> What exactly is Hash Collision - is it a feature, or common phenomenon which is mistakenly done but good to avoid?

It's a feature. It arises out of the nature of a hashCode: a mapping from a large value space to a much smaller value space. There are going to be collisions, by design and intent.&#x20;

> What exactly causes Hash Collision - the bad definition of custom class' hashCode() method,

A bad design can make it worse, but it is endemic in the notion.

> OR to leave the equals() method un-overridden while imperfectly overriding the hashCode() method alone,

No.

> OR is it not up to the developers and many popular java libraries also has classes which can cause Hash Collision?

This doesn't really make sense. Hashes are bound to collide sooner or later, and poor algorithms can make it sooner. That's about it.

> Does anything go wrong or unexpected when Hash Collision happens?

Not if the hash table is competently written. A hash collision only means that the hashCode is not unique, which puts you into calling `equals()`, and the more duplicates there are the worse the performance.

> I mean is there any reason why we should avoid Hash Collision?

You have to trade off ease of computation against spread of values. There is no single black and white answer.

> Does Java generate or at-least try to generate unique hasCode per class during object initiation?

No. 'Unique hash code' is a contradiction in terms.

> If no, is it right to rely on Java alone to ensure that my program would not run into Hash Collision for JRE classes? If not right, then how to avoid hash collision for hashmaps with final classes like String as key?

The question is meaningless. If you're using `String` you don't have any choice about the hashing algorithm, and you are also using a class whose hashCode has been slaved over by experts for twenty or more years.



