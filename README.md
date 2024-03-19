```gleam
import gleam/io
import gleam/option.{type Option, None, Some}

pub type Node {
  Node(data: String, prev: Option(Node), next: Option(Node))
}

pub type Head = Node

pub fn main() {
  let test_1 = insertinbegin("T1", None)
  let test_2 = insertinbegin("T2", Some(test_1.1))
  io.debug(test_2)
}

pub fn insertinbegin(data: String, curr_head: Option(Node)) -> #(Option(Head), Node) {
    case curr_head {
      None -> #(None, Node(data, None, None))
      Some(hd) -> {
        let updated_head: Option(Node) = Some(Node(hd.data, Some(Node(data, None, Some(hd))), hd.next))
        let node = Node(data, None, updated_head)
        #(updated_head, node)
      }
    }
}
```
