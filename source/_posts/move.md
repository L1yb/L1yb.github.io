---
title: move
date: 2022/2/10 22:46:25
---


# bhv_base_move 
 `bool Bhv_BasicMove::execute(PlayerAgent *agent)`
block interpretion   

```cpp
dlog.addText(Logger::TEAM, __FILE__ ": Bhv_BasicMove");

  const WorldModel &wm = agent->world();

  // tackle
  if (Bhv_BasicTackle(0.8, 80.0).execute(agent) &&
      !wm.existKickableTeammate())
  {
    return true;
  }

  Vector2D ball = wm.ball().pos(); //
  Vector2D me = wm.self().pos();

  const Vector2D target_point = Strategy::i().getPosition(wm.self().unum());

  Vector2D homePos = target_point;

  int num = wm.self().unum();

```
question :what's `  const Vector2D target_point = Strategy::i().getPosition(wm.self().unum());
` mean  ? 

The meaning is that:

strategy get the agent's unum ,generating a `target_position`,different from `me`.

`me ` is current position,but `target_position` is the target calculated according to `strategy`.



```
 //对不同位置的球员的Intercept动作分情况判定
  {
    if (num > 5 && !wm.existKickableTeammate() &&
        (self_min <= 3 ||
         (self_min < mate_min + 3 && self_min < opp_min + 4))) {
      Body_Intercept().execute(agent);
      agent->setNeckAction(new Neck_OffensiveInterceptNeck());
      return true;
    }
```
if there are
agent whose unum greater than other teammates and no other kickable teammates and
(the min cycle times of the agent to get target are below 3 or 
	(Within three cycles after the arrival of teammates the agent can get there    and 
	within four cycles after the arrival of opponents 
	) 
) 
execute interception




```cpp
//实现两个中后卫在危险位置对球的拦截
  {
    int goalCycles = 100;

    for (int z = 1; z < 15; ++z)
      if (wm.ball().inertiaPoint(z).x < -52.5 &&
          wm.ball().inertiaPoint(z).absY() < 8.0) {
        goalCycles = z;
        break;
      }

    if ((wm.self().unum() == 2 || wm.self().unum() == 3) && goalCycles != 100 &&
        mate_min >= goalCycles && me.x < -45.0 && !wm.existKickableTeammate()) {
      if (wm.ball().inertiaPoint(self_min).x > -52.0)
        Body_Intercept().execute(agent);
      else
        Body_GoToPoint(wm.ball().inertiaPoint(goalCycles - 1), 0.5,
                       ServerParam::i().maxDashPower())
            .execute(agent);

      agent->setNeckAction(new Neck_OffensiveInterceptNeck());
      return true;
    }
  }

  {
    if (me.x < -37.0 && opp_min < mate_min && opp_min > 2 &&
        homePos.x > -37.0 && wm.ourDefenseLineX() > me.x - 2.5) {
      Body_GoToPoint(rcsc::Vector2D(me.x + 15.0, me.y), 0.5,
                     ServerParam::i().maxDashPower(),
                     ServerParam::i().maxDashPower(), 2, true, 5.0)
          .execute(agent);

      if (wm.existKickableOpponent() && wm.ball().distFromSelf() < 12.0)
        agent->setNeckAction(new Neck_TurnToBall());
      else
        agent->setNeckAction(new Neck_TurnToBallOrScan());
      return true;
    }
  }

```

question: in the begin of code block ,what's `int z`?
then we can check the definition of `inertiaPoint`
```cpp
    Vector2D inertiaPoint( const int cycle ) const
      {
          return inertia_n_step_point( pos(),
                                       vel(),
                                       cycle,
                                       ServerParam::i().ballDecay() );
      }
```
`inertiaPoint`:estimate future point after n steps only by [inertia](  "惯性")(仅通过惯性估计n步后的未来点
)
so this function is to* estimate whether the ball will enter a dangerous area ,if wil,how much cycles.  *

next , judge whether to execute interception.




```cpp
// Defence Half的移动策略。不参与前场进攻
  {
    if (num == 6 && wm.self().stamina() > 3500.0 && ball.x > 33.0) {
      Vector2D opp = wm.opponentsFromSelf().front()->pos();

      float oDist = wm.opponentsFromSelf().front()->pos().dist(homePos);

      if (oDist < 8.0 && opp.x > homePos.x)
        homePos = opp + Vector2D::polar2vector(2.5, (ball - opp).th());

      if (homePos.x > 22.0) homePos.x = 22.0;
    }
  }

  {
    if (wm.self().unum() == 6 && ball.x < 10.0 && ball.absY() > 12.0 &&
        opp_min < mate_min && ball.x > -36.0) {
      Vector2D opp = wm.opponentsFromSelf().front()->pos();
      float oDist = wm.opponentsFromSelf().front()->pos().dist(homePos);

      if (oDist < 8.0) {
        homePos = opp + Vector2D::polar2vector(1.0, (ball - opp).th());
        homePos.x -= 1.0;
        isSoftMarking = true;
      }
    }
  }

  Vector2D tempHomePos = homePos;

```

**critical code**: `wm.opponentsFromSelf().front()->pos()`
**explain**:get opponents sorted by distance from self ,and then get the first opp's pos




[]("")
[vector]("矢量")
[sort]("分类")
[intercept]("拦截")
[inertia]("惯性")