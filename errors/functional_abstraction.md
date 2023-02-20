# Problem encountered

The `self.x += 1` statement appears multiple times. 

``` rust
impl State {
  fn tick(&mut self) {
    self.state = match self.state {
      Ping(s) => { self.x += 1; Pong(s) }
      Pong(s) => { self.x += 1; Ping(s) }
    }
  }
}
```

Why not extract it into a method

```rust
impl State {
  fn tick(&mut self) {
    self.state = match self.state {
      Ping(s) => { self.inc(); Pong(s) } // ← compile error
      Pong(s) => { self.inc(); Ping(s) } // ← compile error
    }
  }

  fn inc(&mut self) {
    self.x += 1;
  }
}
```

Rust will bark at us because the method attempts to re-borrow self exclusively while the surrounding context still holds a mutable reference to `self.state`.
