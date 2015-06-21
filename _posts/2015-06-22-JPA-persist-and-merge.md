---
layout: post
title: "JPA - Persist and merge"
---

# DB insert

Play framework 에 JPA를 이용하는 프로젝트를 진행하고있다. JPA는 경험해본적이 없어서 이래저래 찾아보면서 진행을 하고 있다. 프로젝트를 진행하면서 궁금했던 점은 DB insert 관련 문제다. 필요했던 연산은 DB insert를 수행하는데 DB에 이미 내용이 있으면 insert하지 않고, 없다면 insert하는 것이었다. 관련해서 한번에 쓸수있는 API가 있을까 궁금해서 검색을 해보았다. 보통 persist()를 이용해서 insert를 하는데. merge() 라는 것이 있었다. 이 기회를 통해 persist()와 merge()를 비교해보자.

# persist() and merge()

persist()와 merge() 모두 데이터를 insert하는데 쓰지만, 약간 다른 목적을 위해서 존재한다. merge()는 entity 오브젝트를 DB에 insert하고, 해당 entity 오브젝트를 entity manager에 붙인다. 일반 insert 동작이라고 생각하면된다. 반면에 merge()는 같은 id를 가지고있는 오브젝트를 먼저 찾는다. 만약에 있다면 update를 수행하고 존재하지 않는다면 insert를 수행한다. IF EXIST ~ UPDATE 쿼리를 생각하면 이해가 쉬울것 같다. 단순히 중복없이 insert만 할 목적이라면 merge()를 이용하는 것 보다 persist()를 이용하는것이 성능상으로 유리할것이다. merge는 존재하는지 찾는 연산을 수행하기 때문이다.

# Conclusion

결론적으로는 merge()로 해결할수 있을것이라 생각했지만 merge의 경우 존재한다면 update를 수행하기 때문에 존재할경우 아무연산도 수행하지 않아야 되는 요구사항이랑 맞지 않았다. 결국 persist() 수행하기전에 find()를 이용해서 있는지 확인하고 없다면 persist()를 수행하는 방식으로 수정하여 이용하고 있다.
