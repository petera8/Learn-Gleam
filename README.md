```gleam
import gleam/io
import gleam/option.{type Option, None, Some}

pub type Node {
  Node(data: String, prev: Option(Node), next: Option(Node))
}

pub fn main() {
  let node = insert_at_begin("Node 1", None)
  // io.debug(node)
  let result1 = insert_at_end("Node 2", Some(node))
  io.debug(result1)
  let result2 = insert_at_end("Node 3", Some(result1))
  io.debug(result2)
}

pub type Head = Option(Node)
pub fn insert_at_begin(data: String, head: Head) -> Node {
    case head {
      None ->  Node(data, None, None)
      Some(prev_head) -> {
        let new_next: Head = Some(Node(prev_head.data, Some(Node(data, None, Some(prev_head))), prev_head.next))
        let head: Node = Node(data, None, new_next)
        head
      }
    }
}
pub fn insert_at_end(data: String, head: Head) -> Node {
  case head {
    None -> Node(data, None, None)
    Some(prev_head) -> {
      let last_node = get_last_node(Some(prev_head), None)
      Node(last_node.data, last_node.prev, Some(Node(data, Some(last_node), None)))
    }
  }
}

fn get_last_node(opt_node: Option(Node), prev_node: Option(Node)) -> Node {
   case opt_node {
     None -> case prev_node {
       None -> panic("")
       Some(node) -> node
     }
     Some(node) -> {
       case node.next{
         None -> node
         Some(next_node) -> get_last_node(next_node.next, Some(next_node))
       }
     }
   }
}
```
