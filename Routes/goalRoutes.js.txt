const express = require("express");
const router = express.Router();

const Goal = require("../Models/GoalModel");
const authmiddleware = require("../Middlewares/authMiddleware");


router.post("/create", authmiddleware, async (req, res) => {

  try {

    const { title } = req.body;

    const goal = new Goal({
      title: title,
      user: req.user
    });

    await goal.save();

    res.json({
      success: true,
      data: goal
    });

  } catch (error) {

    res.status(500).json({
      success: false,
      message: error.message
    });

  }

});


router.get("/", authmiddleware, async (req, res) => {

  try {

    const goals = await Goal.find({
      user: req.user
    });

    res.json({
      success: true,
      data: goals
    });

  } catch (error) {

    res.status(500).json({
      success: false,
      message: error.message
    });

  }

});


router.put("/:id", authmiddleware, async (req, res) => {

  try {

    const goal = await Goal.findByIdAndUpdate(
      req.params.id,
      req.body,
      { new: true }
    );

    res.json({
      success: true,
      data: goal
    });

  } catch (error) {

    res.status(500).json({
      success: false,
      message: error.message
    });

  }

});


router.delete("/:id", authmiddleware, async (req, res) => {

  try {

    await Goal.findByIdAndDelete(req.params.id);

    res.json({
      success: true,
      message: "Goal deleted"
    });

  } catch (error) {

    res.status(500).json({
      success: false,
      message: error.message
    });

  }

});


module.exports = router;