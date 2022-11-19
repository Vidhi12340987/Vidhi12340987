


Camera.fixObjectToCamera(Tf2_rocket_launcher, 1, 0, -0.5);

Camera.fixObjectToCamera(Text3D, 0, 2, -4);

var score = 0;

function shoot() {

    Sphere.turnOffPhysics();

    var camX = Camera.getX()
    var camY = Camera.getY()
    var camZ = Camera.getZ()

    Sphere.setPosition(camX, camY, camZ);

    Sphere.setVisible(true)

    Sphere.enablePhysics("dynamic-body")

    var speed = 50;

    var velX = Camera.getGazeDirection().x * speed;
    var velY = Camera.getGazeDirection().y * speed;
    var velZ = Camera.getGazeDirection().z * speed;


    Sphere.setBodyVelocity(velX, velY, velZ);


}


Hatch.onSceneClicked(function (event) {
    shoot();

});

for (var u = 0; u < 25; u++) {
    let x = 7 * Math.floor(10 * (Math.random() - 0.5));
    let z = 7 * Math.floor(10 * (Math.random() - 0.5));
    let y = 8;

    let currentSpark = null;

    Hatch.cloneObject(ParticleSystem, function (clonedObject) {
        clonedObject.setVisible(true);
        clonedObject.setX(x);
        clonedObject.setY(y);
        clonedObject.setZ(z);

        currentSpark = clonedObject;
    })
    Hatch.cloneObject(Drone, function (clonedObject) {
        clonedObject.setVisible(true);
        clonedObject.setX(x);
        clonedObject.setY(y);
        clonedObject.setZ(z);


        clonedObject.OnCollisionWithPhysicsBody(function (hit) {

            Sphere.setVisible(false);
            clonedObject.setVisible(false);
            Gunsound.playSound()
            currentSpark.turnEmitterOn();
            Text3D.setText('Score:' + (++score))
        });

    });

}
