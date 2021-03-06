=========
py-slippi
=========

Py-slippi is a Python parser for `.slp <https://github.com/project-slippi/slippi-wiki/blob/master/SPEC.md>`_ game replay files for `Super Smash Brothers Melee <https://en.wikipedia.org/wiki/Super_Smash_Bros._Melee>`_ for the Nintendo GameCube. These replays are generated by Jas Laferriere's `Slippi <https://github.com/JLaferri/project-slippi>`_ recording code, which runs on a Wii or the `Dolphin <https://dolphin-emu.org/>`_ emulator.

Installation
============

*Requires Python >= 3.6*. To install, run the following command (optionally inside a `virtual environment <https://packaging.python.org/tutorials/installing-packages/#optionally-create-a-virtual-environment>`_):

    pip install py-slippi

Usage
=====

**Reading from a replay file**::

    >>> from slippi import Game
    >>> game = Game('test/replays/game.slp')
    >>> game.metadata
    Metadata(date=2018-06-22 07:52:59+00:00, duration=5086, platform=Platform.DOLPHIN, players=(Player(characters={InGameCharacter.MARTH: 5086}), Player(characters={InGameCharacter.FOX: 5086}), None, None))
    >>> game.start
    Start(is_frozen_ps=None, is_pal=None, is_teams=False, players=(Player(character=CSSCharacter.MARTH, costume=3, stocks=4, tag=, team=None, type=Type.HUMAN, ucf=UCF(dash_back=DashBack.OFF, shield_drop=ShieldDrop.OFF)), Player(character=CSSCharacter.FOX, costume=0, stocks=4, tag=, team=None, type=Type.CPU, ucf=UCF(dash_back=DashBack.OFF, shield_drop=ShieldDrop.OFF)), None, None), random_seed=3803194226, slippi=Slippi(version=1.0.0), stage=Stage.YOSHIS_STORY)
    >>> game.end
    End(lras_initiator=None, method=Method.CONCLUSIVE)
    >>> game.frames[0]
    Frame(index=-123, ports=(Port(follower=None, leader=Data(post=Post(airborne=None, character=InGameCharacter.MARTH, combo_count=0, damage=0.00, direction=Direction.RIGHT, flags=None, ground=None, hit_stun=None, jumps=None, l_cancel=None, last_attack_landed=None, last_hit_by=None, position=(-42.00, 26.60), shield=60.00, state=ActionState.ENTRY, state_age=-1.00, stocks=4), pre=Pre(buttons=Buttons(logical=Logical.NONE, physical=Physical.NONE), cstick=(0.00, 0.00), damage=None, direction=Direction.RIGHT, joystick=(0.00, 0.00), position=(-42.00, 26.60), random_seed=3849336064, raw_analog_x=None, state=ActionState.ENTRY, triggers=Triggers(logical=0.00, physical=Physical(l=0.00, r=37793343381764747296768.00))))), Port(follower=None, leader=Data(post=Post(airborne=None, character=InGameCharacter.FOX, combo_count=0, damage=0.00, direction=Direction.LEFT, flags=None, ground=None, hit_stun=None, jumps=None, l_cancel=None, last_attack_landed=None, last_hit_by=None, position=(42.00, 28.00), shield=60.00, state=ActionState.ENTRY, state_age=-1.00, stocks=4), pre=Pre(buttons=Buttons(logical=Logical.NONE, physical=Physical.NONE), cstick=(0.00, 0.00), damage=None, direction=Direction.LEFT, joystick=(0.00, 0.00), position=(42.00, 28.00), random_seed=3849336064, raw_analog_x=None, state=ActionState.ENTRY, triggers=Triggers(logical=0.00, physical=Physical(l=0.00, r=37793343381764747296768.00))))), None, None))

**Iterating over frame data**::

    for frame in game.frames:
        data = frame.ports[0].leader # see also: port.follower (ICs)
        print(data.post.state) # character's post-frame action state

**Event-driven API**::

    >>> from slippi.parse import parse
    >>> from slippi.event import ParseEvent
    >>> handlers = {ParseEvent.METADATA: print}
    >>> parse('test/replays/game.slp', handlers)
    Metadata(date=2018-06-22 07:52:59+00:00, duration=5209, platform=Platform.DOLPHIN, players=(Player(characters={InGameCharacter.MARTH: 5209}), Player(characters={InGameCharacter.FOX: 5209}), None, None))

API Docs
========

See the `Module Index <https://py-slippi.readthedocs.io/en/latest/py-modindex.html>`_ for detailed API docs, starting with `slippi.game <https://py-slippi.readthedocs.io/en/latest/source/slippi.html#module-slippi.game>`_.
