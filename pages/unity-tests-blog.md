---
layout: post
title: 'UNITY TEST RUNNER BLOG'
author: Thomas Wood
tagline: 'My short exploration into Unity Test Runner'
description: 'My short exploration into Unity Test Runner'
---

<p align="center">
  After spending time being introduced to and writing tests and testable code in Unreal Engine 4 (UE4) at Rare, 
  I thought I would explore the testing framework of the engine I use the most at home - Unity. I picked
  a small scene with some scripts in it that I had written with no intention to test and got to writing a 
  few tests in it while learning what Unity Test Runner had to offer. The scene I had to test was a simple 
  2D Asteroids-like shooter and the tests I decided to write centre around the bullet. The functionality of the bullet I started by
  testing was the BulletMove component which runs this <i>Move()</i> function every frame:
</p>
```
void Move()
{
  Vector3 positionChange = transform.up * moveSpeed * Time.deltaTime;
  transform.position += positionChange;
}
```
<p align="center">
  The BulletMove tests came first. It didn't take long to get started - the framework feels very familar after working in UE4's. It also
  introduced me to <i>Assembly Definitions</i> in Unity for the first time, which seem like a must for compile times in large Unity 
  projects a la UE4's modules. The first test I wrote for BulletMove was as follows:
</p>
```
[UnityTest]
public IEnumerator TEST_Bullet_Moves_Expected_Distance_GIVEN_Move_Speed()
{
  // Set-up bullet
  bullet = MonoBehaviour.Instantiate(Resources.Load<GameObject>("Bullet"));
  bulletMove = bullet.GetComponent<BulletMove>();
  bulletMove.moveSpeed = 10.0f;
      
  Vector3 initialPosition = bullet.transform.position;

  // Move bullet
  const float passedTime = 0.1f;
  yield return new WaitForSeconds(passedTime);
      
  Vector3 newPosition = bullet.transform.position;

  float moveMagnitude = (newPosition - initialPosition).magnitude;

  // Assert bullet moved expected distance with a small tolerance
  Assert.AreEqual(bulletMove.moveSpeed * passedTime, moveMagnitude, 0.1f);
            
  // Clean-up bullet
  bulletMove = null;
  Object.Destroy(bullet);
}
```
<p align="center">
  After making sure the test failed and then passed, I started on the second test. At this point it made sense to pull out the set-up 
  and clean-up of the first test into new functions, knowing this would be the same for the both tests. Instead of manually calling the 
  set-up and clean-up functions myself at the start and end of every test, I was glad to see Unity Test Runner had mark-up which would
  have the functions called automatically:
</p>
```
// Called before every test
[SetUp]
public void SetUp()
{
  bullet = MonoBehaviour.Instantiate(Resources.Load<GameObject>("Bullet"));
  bulletMove = bullet.GetComponent<BulletMove>();
}

// Called after every test
[TearDown]
public void TearDown()
{
  bulletMove = null;
  Object.Destroy(bullet);
}
```
<p align="center">
  I then wrote the second test, which makes sure the :
</p>
```
[UnityTest]
public IEnumerator TEST_Bullet_Moves_Expected_Direction_GIVEN_Rotation()
{
  bullet.transform.Rotate(Vector3.forward, 45.0f);
  bulletMove.moveSpeed = 1.0f;

  Vector3 initialPosition = bullet.transform.position;

  const float passedTime = 1.0f;
  yield return new WaitForSeconds(passedTime);

  Vector3 newPosition = bullet.transform.position;

  Vector3 moveDirection = newPosition - initialPosition;
            
  Assert.AreEqual(-0.7f, moveDirection.x, 0.01f, "X position");
  Assert.AreEqual(0.7f, moveDirection.y, 0.01f, "Y position");
  Assert.AreEqual(0.0f, moveDirection.z, "Z position");
}
```
