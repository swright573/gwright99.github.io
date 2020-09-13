like:
* Todd Birchard's [Building a Python App in Flask](https://hackersandslackers.com/series/build-flask-apps/)
* Miguel Grinberg's [Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)

#### How about you stop digressing and get to the point?
Ok ok, my beef with ORMs is centred around the following 4 points:
1. Registering the ORM when using the Flask Application Factory (tight coupling)
2. Intermingling SQL abstractions with object relationships
3. Obfuscation of underlying database interactions
4. Learning the ORM rather than learning SQL