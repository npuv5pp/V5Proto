# protocol buffer基本数据结构

```protobuf
enum Version {
    V1_0 = 0;
    V1_1 = 1;
}

enum Team {
    Self = 0;
    Opponent = 1;
    Nobody = 2;
}

message Vector2 {
    float x = 1;
    float y = 2;
}

message TeamInfo {
    string team_name = 1;
    Version version = 2;
}

message Ball {
    Vector2 position = 1;
}

message Wheel {
    float left_speed = 1;
    float right_speed = 2;
}

message Robot {
    Vector2 position = 1;
    float rotation = 2;
    Wheel wheel = 3;
}

message Field {
    repeated Robot self_robots = 1;
    repeated Robot opponent_robots = 2;
    Ball ball = 3;
    int32 tick = 4;
}

message Placement { 
    repeated Robot robots = 1;
    Ball ball = 2;
}

enum ControlType {
    Continue = 0;
    Reset = 1;
}

message ControlInfo {
    ControlType command = 1;
}
```
上述字段含义如下

## TeamInfo
- **team_name**：己方队伍名。
- **version**：[仿真平台](https://github.com/npuv5pp/Simuro5v5)版本号。

## Wheel
- **left_speed**：左轮速（-125 ~ 125）。
- **right_speed**：右轮速（-125 ~ 125）。

## Robot
- **position**：球员位置。
- **rotaion**：球员指向（采用角度制，取值为-180 ~ 180，逆时针角度减小，顺时针角度增大，在[右攻假设](https://github.com/npuv5pp/Simuro5v5/blob/master/README_ZH.md#%E5%8F%B3%E6%94%BB%E5%81%87%E8%AE%BE)的前提下，正右方始终为0°）。
- **wheel**：球员轮速

## Field
- **self_robots**：己方球员序列（要求长度为5）。
- **opponent_robots**：对方球员序列（要求长度为5）。
- **ball**：足球对象。
- **tick**：比赛时间（单位为1/66秒）。

## Placement
- **robots**：己方球员摆位（要求为长度为5的数组）。
- **ball**：球的摆位。

## ControlType
- **Continue**：比赛继续。
- **Reset**：重新摆位。

## ControlInfo
- **command**：控制比赛启停。

# protocol buffer事件定义

可能的事件类型如下：

```protobuf
enum EventType {
    JudgeResult = 0;
    MatchStart = 1;
    MatchStop = 2;
    FirstHalfStart = 3;
    SecondHalfStart = 4;
    OvertimeStart = 5;
    PenaltyShootoutStart = 6;
}
```

- **JudgeResult**：当平台公布裁判结果时发送。参数类型为`JudgeResultEvent`。
- **MatchStart**：当比赛开始时发送。没有参数。
- **MatchStop**：当比赛结束时发送。没有参数。
- **FirstHalfStart**：当上半场开始时发送。没有参数。
- **SecondHalfStart**：当下半场开始时发送。没有参数。
- **OvertimeStart**：当加时赛开始时发送。没有参数。
- **PenaltyShootoutStart**：当点球大战开始时发送。没有参数。

可能的事件参数如下：

## JudgeResultEvent
```protobuf
message JudgeResultEvent {
    enum ResultType {
        PlaceKick = 0;
        GoalKick = 1;
        PenaltyKick = 2;
        FreeKickRightTop = 3;
        FreeKickRightBot = 4;
        FreeKickLeftTop = 5;
        FreeKickLeftBot = 6;
    }

    ResultType type = 1;
    Team offensive_team = 2;
    string reason = 3;
}
```
上述字段含义[参考](https://docs.google.com/document/d/1yrqu5rSUCoFQsl0ct5l-9B2dFvcIQqlfMA5UQ4WIaGo/edit#heading=h.3tbugp1)。

# 作者
该项目当前由 西北工业大学V5++团队 编写和维护。保留相应权利。
