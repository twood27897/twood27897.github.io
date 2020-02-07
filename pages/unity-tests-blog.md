---
layout: post
title: 'UNITY TEST RUNNER BLOG'
author: Thomas Wood
tagline: 'My short exploration into Unity Test Runner'
description: 'My short exploration into Unity Test Runner'
---

<p align="center">
  After spending time being introduced to and writing tests and testable code in Unreal Engine 4 (UE4) at Rare, I thought I would explore the testing framework of the engine I use the most at home - Unity. I picked a small scene with some scripts in it that I had written with no intention to test and got to writing a few tests in it while learning what <a href="https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/index.html" rel="Unity Test Framework/Runner">Unity Test Framework/Runner</a> has to offer. The scene I had to test was a simple 2D Asteroids-like shooter and the tests I decided to write centre around the bullet's movement. This functionality of the bullet is contained in the BulletMove component which runs this <i>Move()</i> function every frame:
</p>
```
void Move()
{
  Vector3 positionChange = transform.up * moveSpeed * Time.deltaTime;
  transform.position += positionChange;
}
```
<p align="center">
  It didn't take long to get started - the framework feels very familiar after working in UE4's. It also introduced me to <i>Assembly Definitions</i> in Unity for the first time, which seem like a must for compile times in large Unity projects a la UE4's modules. The first test I wrote for BulletMove was as follows:
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
  After making sure the test failed and then passed, I started on the second test. At this point it made sense to pull out the set-up and clean-up of the first test into new functions as I knew this would be the same for the both tests. Rather than manually calling the set-up and clean-up functions myself at the start and end of every test, I was glad to see Unity Test Framework/Runner had mark-up which would have the functions called automatically:
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
  I then wrote the second test, which makes sure the bullet moves in the correct direction based on it's rotation:
</p>
```
[UnityTest]
public IEnumerator TEST_Bullet_Moves_Expected_Direction_GIVEN_Rotation()
{
  bullet.transform.Rotate(Vector3.forward, 45.0f);
  bulletMove.moveSpeed = 10.0f;

  Vector3 initialPosition = bullet.transform.position;

  const float passedTime = 0.1f;
  yield return new WaitForSeconds(passedTime);

  Vector3 newPosition = bullet.transform.position;

  Vector3 moveDirection = newPosition - initialPosition;
            
  Assert.AreEqual(-0.7f, moveDirection.x, 0.01f, "X position");
  Assert.AreEqual(0.7f, moveDirection.y, 0.01f, "Y position");
  Assert.AreEqual(0.0f, moveDirection.z, "Z position");
}
```
<p align="center">
  Much nicer now we don't have the set-up and clean-up code in there! At this point the tests were taking longer than I'd like to complete, with having to yield for the <i>Update()</i> function to run being the main culprit. The two tests were taking <i>0.127s</i> and <i>0.124s</i> respectively. Most of that time is the <i>0.1s</i> yield:
</p>
<img border="0" alt="UnityTest Times" src="http://twood27897.github.io/assets/UnityTestTimesTogether.png" width="428" height="172">
<p align="center">
  I wasn't a fan of the idea of reducing the yield or increasing the time scale just to get the test to run faster as I thought that was not representative of the scenes normal state. So, I took a look around and did a small refactor on my BulletMove code. Instead of making the <i>Move()</i> function access the global <i>Time</i> namespace itself I passed the delta time into the function as a parameter. This meant in the tests I didn't have to yield at all, I could just call <i>Move()</i> directly with whatever time I wanted to pretend had passed. At this point I could also make the tests use the normal NUnit <i>Test</i> attribute instead of <i>UnityTest</i>. The difference between the <i>UnityTest</i> and <i>Test</i> attributes are as follows: 
<ul>
  <li><i>UnityTest</i> allows you to run tests which require time to pass in Unity's playmode - for instance to see if collision responses are correct or to make sure the <i>Update()</i> does what you expect.</li>
  <li><i>Test</i> allows you to run simple tests without yielding - more like a normal, barebones unit test.</li>
</ul> 
  With the yields gone from the tests their times were down to <i>0.05s</i> and <i>0.001s</i>:
</p>
<img border="0" alt="Test Times" src="http://twood27897.github.io/assets/TestTimesTogether.png" width="428" height="172">
<p align="center">
  At this point I was happy with my tests and what I'd learned of the Unity Test Framework/Runner. Overall, at a glance, tests seem to work and have the capabilities I would have expected. I think if I was to continue writing tests in Unity I would use a combination of both <i>UnityTest</i>s and <i>Test</i>s, as would be expected. I would definitely focus the full, yieldable tests on objects which regularly had issues or caused bugs, writing them in post and trying to keep them sparse so they wouldn't bloat the test times. I'd also test multiple things in a single <i>UnityTest</i> considering the times it can take - with this movement code for example I'd have a single test yielding so the <i>Update()</i> of the object is called, and then test both the move distance and direction were the expected results. It was also nice to see my experience writing tests at Rare coming into play as I felt very comfortable using Unity Test Framework/Runner. Nice experience and something I will be considering for future projects.
</p>
