---
layout: post
title: Diamond Kata With Elixir
modified:
categories:
excerpt: My first steps in elixir with the diamond kata.
tags: [coding elixir]
image:
  feature:
date: 2016-02-21T21:16:47+01:00
---
During my parental leave I started to have a look at [elixir][1], a functional language on the erlang VM. It is inspired by ruby and comes with constructs like actors for parallel programming.

Katas are a great way to learn languages so I used the [diamond kata][2] to dive a bit deeper into elixir. I worked the iterative way through the kata, with a long period of thinking since I didn´t know to start. Then I decided to start calculating the tip, the middle blanks and finally the diamond.

Random thoughts:

* It´s important to be able to calculate the positions of the letters
* I used the codepoint of the letters - and avoided therefore the declaration of all existing letters
* I´ve created the upper part of the diamond and mirrored it afterwards
* The middle whitespaces are always the same regardless of the size of the diamond
* [Code on GitHub][3]

{% codeblock lang:elixir %}

defmodule DiamondTest do
  use ExUnit.Case
  doctest ElixirPlayground

  test "calculate the tip" do
    assert tip("A") == "A"
    assert tip("B") == " A "
    assert tip("C") == "  A  "
  end

  def test "middle_blanks" do
    assert middle_blanks("B") == 1
    assert middle_blanks("C") == 3
    assert middle_blanks("D") == 5
  end

  test "position" do
    assert line_for("A", "C") == "  A  "
    assert line_for("B", "C") == " B B "
    assert line_for("B", "D") == "  B B  "
    assert line_for("C", "D") == " C   C "
    assert line_for("C", "E") == "  C   C  "
    assert line_for("C", "F") == "   C   C   "
    assert line_for("F", "F") == "F         F"
  end

  test "half" do
      assert half("B") == [" A ", "B B"]
      assert half("C") == ["  A  ", " B B ", "C   C"]
      assert half("D") == ["   A   ", "  B B  ", " C   C ", "D     D"]
  end

  test "diamond" do
      assert diamond("C") == "  A  \n B B \nC   C\n B B \n  A  "
      assert diamond("D") == "   A   \n  B B  \n C   C \nD     D\n C   C \n  B B  \n   A   "
  end

  def diamond(letter) do
    upper_part = half(letter)
    lower_part = List.delete_at(Enum.reverse(half(letter)),0)
    Enum.join(upper_part ++ lower_part, "\n")
  end

  def half(diamond_letter) do
      the_list = Enum.to_list((int_codepoint("A")..int_codepoint(diamond_letter)))
      Enum.map(the_list, &(line_for(<<&1 :: utf8>>, diamond_letter)))
  end

   def line_for("A", diamond_letter) do
     tip(diamond_letter)
   end

   def line_for(current_letter, diamond_letter) do
     border = border_blanks(current_letter, diamond_letter)
     middle = middle_blanks(current_letter)

     border <> current_letter <> middle <> current_letter <> border
  end

  def int_codepoint(letter) do
      <<codepoint, 0>> = letter <> <<0>>
      codepoint
  end

  def border_blanks(current_letter, diamond_letter)do
    current = int_codepoint(current_letter)
    diamond = int_codepoint(diamond_letter)

    blank(diamond - current)
  end

  def middle_blanks(letter) do
    start = int_codepoint("A")
    current = int_codepoint(letter)
    blank((current - start) * 2 - 1)
  end

  def blank(number) do
    String.duplicate(" ", number)
  end

  defp tip(diamond_letter) do
    <<x,0, y>> = diamond_letter <> <<0>> <> "A"
    padding = String.duplicate(" ", x - y)
    padding <> "A" <> padding
  end
end

{% endcodeblock %}


[1]: http://elixir-lang.org/
[2]: http://www.dyalog.com/blog/2015/01/the-diamond-kata/
[3]: https://github.com/thilko/elixir_playground/blob/master/test/diamond_test.exs
