# ECS(Entity, Component, System)
Componnet 是定义游戏世界各种概念的`基本单元`，比如代表玩家的 Player, 代表位置的 Position, 代表计时器的 Timer，代表窗口的 Window 等；
Component 可以是任意 struct 或 enum


Entity 是游戏运行时一组相关 `Component 的集合`，比如描述一个玩家精灵时，需要 (Player, Position) 两个 Component 来标识；
Entity 可以通过 Commands::spawn 生成


System 定义游戏运行的`规则`，即 Entity 间如何交互。

```

// A system
fn setup_system(
    mut commands: Commands
) {
    // Create Entity
    commands.spawn()
        .insert(Player {name: "player1".to_string()})
        .insert(Position {x: 0.0, y: 0.0});

    // Create Entity
    commands.spawn()
        .insert(Player {name: "player2".to_string()})
        .insert(Position {x: 0.0, y: 0.0});
}

fn debug_system(
    mut user_query: Query<(&Player, &mut Position)>,
) {
    println!("debug_system 开始");
    for (player, mut position) in user_query.iter_mut() {
        println!("查询到 {}, 当前位置： {},{}", player.name, position.x, position.y);
        println!("更新 {}", player.name);
        position.x += 1.0;
        position.y += 1.0;
    }
    println!("debug_system 结束\n\n");
}

fn main() {
    App::build()
        .add_plugins(DefaultPlugins)
        .add_startup_system(setup_system.system()) // <-- setup_system 只需要运行一次
        .add_system(debug_system.system())   // <-- debug_system 在游戏运行的每个物理帧都要执行
        .run();
}
```