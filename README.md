
# Motion
My Japanese learning motion graphics video
import React from "react";
import {AbsoluteFill} from "remotion";
import {TransitionSeries, linearTiming} from "@remotion/transitions";
import type {TransitionPresentation} from "@remotion/transitions";
import {fade} from "@remotion/transitions/fade";
import {slide} from "@remotion/transitions/slide";
import {wipe} from "@remotion/transitions/wipe";
import {Background} from "./components";
import {D, TRANSITION} from "./data";
import {Opening} from "./scenes/Opening";
import {WordScene} from "./scenes/WordScene";
import {HiraganaScene} from "./scenes/HiraganaScene";
import {RomajiScene} from "./scenes/RomajiScene";
import {MeaningScene} from "./scenes/MeaningScene";
import {ExampleScene} from "./scenes/ExampleScene";
import {ExplainScene} from "./scenes/ExplainScene";
import {RecapScene} from "./scenes/RecapScene";
import {EndingScene} from "./scenes/EndingScene";

const SCENES: {Comp: React.FC; d: number}[] = [
  {Comp: Opening, d: D.opening},
  {Comp: WordScene, d: D.word},
  {Comp: HiraganaScene, d: D.hiragana},
  {Comp: RomajiScene, d: D.romaji},
  {Comp: MeaningScene, d: D.meaning},
  {Comp: ExampleScene, d: D.example},
  {Comp: ExplainScene, d: D.explain},
  {Comp: RecapScene, d: D.recap},
  {Comp: EndingScene, d: D.ending},
];

const PRESENTATIONS: TransitionPresentation<any>[] = [
  fade(),
  slide({direction: "from-right"}),
  wipe({direction: "from-left"}),
  slide({direction: "from-bottom"}),
];

export const Video: React.FC = () => {
  const nodes: React.ReactNode[] = [];
  SCENES.forEach((s, i) => {
    if (i > 0) {
      nodes.push(
        <TransitionSeries.Transition
          key={`t${i}`}
          presentation={PRESENTATIONS[i % PRESENTATIONS.length]}
          timing={linearTiming({durationInFrames: TRANSITION})}
        />
      );
    }
    nodes.push(
      <TransitionSeries.Sequence key={`s${i}`} durationInFrames={s.d}>
        <s.Comp />
      </TransitionSeries.Sequence>
    );
  });

  return (
    <AbsoluteFill>
      <Background />
      <TransitionSeries>{nodes}</TransitionSeries>
    </AbsoluteFill>
  );
};
