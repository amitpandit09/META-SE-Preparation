**Question**

Leetcode https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses

// VARIANT: What if you were given different types of parentheses?

// Each type of parenthesis is balanced independently of the others.

// Otherwise, this might explode into a DP problem and Meta swear to never ask 

// Dynamic programming. They'd never lie, right?

**Clarifying questions**

1. Input is string? yes, output is string? yes
2. 0-9, a-z lowercase ? yes
3. Can string be empty ? valid
4. Will there be only {}, (), [] not <> ?
5. 1 <= s.length <= 10^5
6. Input will contain ASCII and not unicode letters
7. Relative order of the characters is maintained

**Solution**

```
import java.util.*;

public class DeleteLeastParentheses {

    public static String deleteLeastParentheses(String s) {
        Map<Character, Character> mapping = new HashMap<>();
        mapping.put(')', '(');
        mapping.put(']', '[');
        mapping.put('}', '{');

        Map<Character, Integer> extraOpens = new HashMap<>();
        Map<Character, Integer> totalOpens = new HashMap<>();
        StringBuilder temp = new StringBuilder();

        for (char ch : s.toCharArray()) {
            if (mapping.containsKey(ch)) { // Closing parentheses
                char open = mapping.get(ch);
                if (extraOpens.getOrDefault(open, 0) == 0) {
                    continue;
                }
                extraOpens.put(open, extraOpens.get(open) - 1);
                temp.append(ch);
            } else if (Character.isLetterOrDigit(ch)) { // 'a' or '3'
                temp.append(ch);
            } else { // Opening parentheses
                extraOpens.put(ch, extraOpens.getOrDefault(ch, 0) + 1);
                totalOpens.put(ch, totalOpens.getOrDefault(ch, 0) + 1);
                temp.append(ch);
            }
        }

        // Determine how many of each opening bracket to keep
        Map<Character, Integer> keep = new HashMap<>();
        for (char open : totalOpens.keySet()) {
            keep.put(open, totalOpens.get(open) - extraOpens.getOrDefault(open, 0));
        }

        StringBuilder result = new StringBuilder();
        for (char ch : temp.toString().toCharArray()) {
            if (totalOpens.containsKey(ch)) { // Opening parentheses
                if (keep.get(ch) == 0) continue;
                keep.put(ch, keep.get(ch) - 1);
                result.append(ch);
            } else {
                result.append(ch);
            }
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Test cases
        assert deleteLeastParentheses("[lee(t(c)o))))d[[e)(({{}}}").equals("lee(t(c)o)de{{}}");
        assert deleteLeastParentheses("(()))))minmer((((()([][[{{}").equals("(())minmer()[]{}");
        assert deleteLeastParentheses("(()))()").equals("(())()");
        assert deleteLeastParentheses("{[({)]}}").equals("{[({)]}}");
        assert deleteLeastParentheses(")))").isEmpty();
        assert deleteLeastParentheses("((((").isEmpty();
        assert deleteLeastParentheses("({({([}").equals("{}");
        assert deleteLeastParentheses("([)]").equals("([)]");
        assert deleteLeastParentheses("([)").equals("()");
        assert deleteLeastParentheses("))((ab()c)(").equals("((ab)c)");
        assert deleteLeastParentheses("((ab((()))c)(").equals("((ab(()))c)");

        System.out.println("All tests passed!");
    }
}

```
