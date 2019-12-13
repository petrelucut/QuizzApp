<template>
    <!--container-->
    <section class="container">

        <div class="questionBox" id="app" v-if="!start">


                <div class="questionContainer" v-if="questionIndex<quiz.questions.length" v-bind:key="questionIndex">

                    <header style="position: absolute; top: 0; right: 0; left: 0;">
                        <h1 class="title is-6" style="">DEV 9 Quiz</h1>

                    </header>

                    <h2 class="titleContainer title">You have 10 minutes to complete the quiz</h2>

                    <div class="optionContainer">
                        <input class="option is-selected" v-model="name" placeholder="Yor name" style="border-radius: unset">

                    </div>

                    <footer class="questionFooter" style="position: absolute; bottom: 0;">

                        <nav class="pagination" role="navigation" aria-label="pagination" style="display: flex; justify-content: center;">

                            <a class="button" v-on:click="startQuiz" :class="{'is-active': name}">
                                Start Quiz
                            </a>

                        </nav>

                    </footer>

                </div>

                <div v-if="questionIndex >= quiz.questions.length" v-bind:key="questionIndex"
                     class="quizCompleted has-text-centered">

                    <span class="icon">
                <i class="fa" :class="score()>3?'fa-check-circle-o is-active':'fa-times-circle'"></i>
              </span>

                    <h2 class="title">
                      Total score: {{ score() }} / {{ quiz.questions.length}}
                    </h2>
                    <p class="subtitle">
                       {{ score() > 6 ? 'Great!' : 'Better luck next time ;)'}}
                    </p>
                    <br>

                </div>

        </div>
        <div class="questionBox" id="app" v-if="start">

            <transition :duration="{ enter: 500, leave: 300 }" enter-active-class="animated zoomIn"
                        leave-active-class="animated zoomOut" mode="out-in">

                <div class="questionContainer" v-if="questionIndex<quiz.questions.length" v-bind:key="questionIndex">

                    <header>
                        <h1 class="title is-6">Dev 9 quiz</h1>
                        <div class="progressContainer">
                            <progress class="progress is-info is-small"
                                      :value="(questionIndex/quiz.questions.length)*100" max="100">
                                {{(questionIndex/quiz.questions.length)*100}}%
                            </progress>
                            <p>{{(questionIndex/quiz.questions.length)*100}}% complete</p>
                        </div>
                    </header>

                    <h2 class="titleContainer title">{{ quiz.questions[questionIndex].question }}</h2>

                    <div class="optionContainer">
                        <div class="option" v-for="(response, index) in quiz.questions[questionIndex].posibleAnswers"
                             @click="selectOption(index, quiz.questions[questionIndex].question, response)"
                             :class="{ 'is-selected': userResponses[questionIndex] == index}" :key="index">
                            {{ index | charIndex }}. {{ response }}
                        </div>
                    </div>

                    <footer class="questionFooter">

                        <nav class="pagination" role="navigation" aria-label="pagination">

                            <a class="button" v-on:click="prev();" :disabled="questionIndex < 1">
                                Back
                            </a>
                            <a class="button" :class="(userResponses[questionIndex]==null)?'':'is-active'"
                               v-if="questionIndex < 9"
                               v-on:click="next();" :disabled="questionIndex>=quiz.questions.length">
                                Next
                            </a>
                            <a class="button"
                               :class="{'is-active' : userResponses[questionIndex] !=null}"
                               v-if="questionIndex == 9"
                               v-on:click="submitAnswers">
                                Finish Quiz
                            </a>

                        </nav>
                    </footer>

                </div>

                <div v-if="questionIndex >= quiz.questions.length" v-bind:key="questionIndex"
                     class="quizCompleted has-text-centered">

                    <span class="icon">
                <i class="fa" :class="score()>3?'fa-check-circle-o is-active':'fa-times-circle'"></i>
              </span>

                    <h2 class="title">
                        Total score: {{ score() }} / {{ quiz.questions.length}}
                    </h2>
                    <p class="subtitle">
                        {{ score() > 6 ? 'Great!' : 'Better luck next time ;)'}}
                    </p>
                    <br>

                </div>

            </transition>

        </div>

    </section>
</template>

<script>
    var quiz = {
            user: "Dave",
            questions: [
                {
                    posibleAnswers: [
                        "<heading>",
                        "<head>",
                        "<h6>",
                        "<h1>"
                    ],
                    question: "Choose the correct HTML element for the largest heading:",
                     correctAnswer: "<h1>",
                     createdAt: "2019-12-09T16:54:53.905Z",
                     updatedAt: "2019-12-09T16:54:53.905Z",
                     id: "5dee7c5d923f372c31649ae8"
                },
                {
                    posibleAnswers: [
                        "<a name='http://www.google.com'>Google.com </a>",
                        "<a href='http://www.google.com'>Google </a>",
                        "<a url='http://www.google.com'>Google.com </a>",
                        "<a >http://www.google.com</a>"
                    ],
                    question: "What is the correct HTML for creating a hyperlink?",
                     correctAnswer: "<a href='http://www.google.com'>Google </a>",
                     createdAt: "2019-12-10T08:42:06.604Z",
                     updatedAt: "2019-12-10T08:42:06.604Z",
                     id: "5def5a5ea085b43b4e837a86"
                },
                {
                    posibleAnswers: [
                        "/",
                        "<",
                        "*",
                        "^"
                    ],
                    question: "Which character is used to indicate an end tag?",
                     correctAnswer: "/",
                     createdAt: "2019-12-10T08:42:52.257Z",
                     updatedAt: "2019-12-10T08:42:52.257Z",
                     id: "5def5a8ca085b43b4e837a87"
                },
                {
                    posibleAnswers: [
                        "<image src='image.gif' alt='MyImage'>",
                        "<img alt='MyImage'>image.gif</img>",
                        "<img src='image.gif' alt='MyImage'>",
                        "<img href='image.gif' alt='MyImage'>"
                    ],
                    question: "What is the correct HTML for inserting an image?",
                     correctAnswer: "<img src='image.gif' alt='MyImage'>",
                     createdAt: "2019-12-10T08:44:22.919Z",
                     updatedAt: "2019-12-10T08:44:22.919Z",
                     id: "5def5ae6a085b43b4e837a88"
                },
                {
                    posibleAnswers: [
                        "<link rel='stylesheet' type='text/css' href='mystyle.css'>",
                        "<stylesheet> mystyle.css </stylesheet>",
                        "<style src='mystyle.css'>",
                        "<stylet> mystyle.css </style>"
                    ],
                    question: "What is the correct HTML for referring to an external style sheet?",
                     correctAnswer: "<link rel='stylesheet' type='text/css' href='mystyle.css'>",
                     createdAt: "2019-12-10T08:46:07.474Z",
                     updatedAt: "2019-12-10T08:46:07.474Z",
                     id: "5def5b4fa085b43b4e837a89"
                },
                {
                    posibleAnswers: [
                        "<p style='font-size: bold'>",
                        "<p style='text-size: bold'>",
                        "p { font-weight: bold }",
                        "p { text-size: bold }"
                    ],
                    question: "What is the correct CSS syntax for making all the <p> elements bold?",
                     correctAnswer: "p { font-weight: bold }",
                     createdAt: "2019-12-10T08:47:50.074Z",
                     updatedAt: "2019-12-10T08:47:50.074Z",
                     id: "5def5bb6a085b43b4e837a8a"
                },
                {
                    posibleAnswers: [
                        "*demo",
                        "demo",
                        ".demo",
                        "#demo"
                    ],
                    question: "How do you select an element with id 'demo'?",
                     correctAnswer: "#demo",
                     createdAt: "2019-12-10T08:48:58.104Z",
                     updatedAt: "2019-12-10T08:48:58.104Z",
                     id: "5def5bfaa085b43b4e837a8b"
                },
                {
                    posibleAnswers: [
                        "*demo",
                        "demo",
                        ".demo",
                        "#demo"
                    ],
                    question: "How do you select an element with class 'demo'?",
                     correctAnswer: ".demo",
                     createdAt: "2019-12-10T08:49:20.471Z",
                     updatedAt: "2019-12-10T08:49:20.471Z",
                     id: "5def5c10a085b43b4e837a8c"
                },
                {
                    posibleAnswers: [
                        "body:color=black;",
                        "body {color: black;}",
                        "{body;color:black;}",
                        "{body: color = black;}"
                    ],
                    question: "Which is the correct CSS syntax?",
                     correctAnswer: "body {color: black;}",
                     createdAt: "2019-12-10T09:06:29.276Z",
                     updatedAt: "2019-12-10T09:06:29.276Z",
                     id: "5def6015a085b43b4e837a96"
                },
                {
                    posibleAnswers: [
                        "div p",
                        "div + p",
                        "div.p",
                        "div > p"
                    ],
                    question: "How do you select all p elements inside a <div> element?",
                     correctAnswer: "div p",
                     createdAt: "2019-12-10T09:08:10.647Z",
                     updatedAt: "2019-12-10T09:08:10.647Z",
                     id: "5def607aa085b43b4e837a97"
                }
            ]
        },
        userResponseSkelaton = Array(quiz.questions.length).fill(null);
        var recordsRkeleton = Array(quiz.questions.length).fill({question: '', answer: ''});

        import axios from 'axios';
    export default {
        name: "Quiz",
        data() {
            return {

                quiz: quiz,
                questionIndex: 0,
                userResponses: userResponseSkelaton,
                isActive: false,
                correctAnswers: [3, 1, 0, 2, 0, 2, 3, 2, 1, 0],
                records: recordsRkeleton,
                name: '',
                start: false,
                timer: {
                    minutes: '5',
                    seconds: '00'
                }
            }
        },
        filters: {
            charIndex: function (i) {
                return String.fromCharCode(97 + i);
            }
        },
        methods: {
            startQuiz() {
                if(this.name) this.start = true;
            },
            selectOption(index, question, answer) {
                this.$set(this.userResponses, this.questionIndex, index);
                this.$set(this.records, this.questionIndex, {question: question, answer: answer})
            },
            next() {
                if (this.userResponses[this.questionIndex] !== null)
                    this.questionIndex++;
            },

            prev() {
                if (this.questionIndex > 0) this.questionIndex--;
            },
            // Return "true" count in userResponses
            score() {
                var score = 0;
                for (let i = 0; i < this.userResponses.length; i++) {
                    if (this.userResponses[i] == this.correctAnswers[i]) {
                        score = score + 1;
                    }
                }
                return score;
            },
            submitAnswers() {
                let url = 'https://quiz-app-backend-segmente.herokuapp.com/app/userRecords';
                let data = {
                    name: this.name,
                    records: this.records,
                    finalScore: this.score()
                };
                let config = {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                }
                if (this.userResponses[this.questionIndex] != null) {
                    axios.post(url, data, config)
                        .then(response => {
                            window.console.log(response);
                            this.questionIndex++;
                        })
                        .catch(error => {
                            alert('Error submitting answers. Please contact your mentor.')
                            window.console.log(error);
                        })
                }
            }
        }
    }
</script>

<style lang="scss">
    $trans_duration: 0.3s;
    $primary_color: #3D5AFE;

    @import url("https://fonts.googleapis.com/css?family=Montserrat:400,400i,700");
    @import url("https://fonts.googleapis.com/css?family=Open+Sans:400,400i,700");

    body {
        font-family: "Open Sans", sans-serif;
        font-size: 14px;

        height: 100vh;

        background: #CFD8DC;

        /* mocking native UI */
        cursor: default !important; /* remove text selection cursor */
        user-select: none; /* remove text selection */
        user-drag: none; /* disbale element dragging */

        display: flex;
        align-items: center;
        justify-content: center;
    }

    .button {
        transition: $trans_duration;
    }

    .title,
    .subtitle {
        font-family: Montserrat, sans-serif;
        font-weight: normal;
    }

    .animated {
        transition-duration: $trans_duration/2;
    }

    .container {
        margin: 0 0.5rem;
    }

    .questionBox {

        max-width: 30rem;
        width: 30rem;
        min-height: 30rem;

        background: #FAFAFA;
        position: relative;
        display: flex;

        border-radius: 0.5rem;
        overflow: hidden;
        box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);

        header {
            background: rgba(0, 0, 0, 0.025);
            padding: 1.5rem;
            text-align: center;
            border-bottom: 1px solid rgba(0, 0, 0, 0.1);

            h1 {
                font-weight: bold;
                margin-bottom: 1rem !important;
            }

            .progressContainer {
                width: 60%;
                margin: 0 auto;

                > progress {
                    margin: 0;
                    border-radius: 5rem;
                    overflow: hidden;
                    border: none;

                    color: $primary_color;

                    &::-moz-progress-bar {
                        background: $primary_color;
                    }

                    &::-webkit-progress-value {
                        background: $primary_color;
                    }
                }

                > p {
                    margin: 0;
                    margin-top: 0.5rem;
                }
            }
        }

        .titleContainer {
            text-align: center;
            margin: 0 auto;
            padding: 1.5rem;

        }

        .quizForm {
            display: block;
            white-space: normal;

            height: 100%;
            width: 100%;

            .quizFormContainer {
                height: 100%;
                margin: 15px 18px;

                .field-label {
                    text-align: left;
                    margin-bottom: 0.5rem;
                }
            }
        }

        .quizCompleted {
            width: 100%;
            padding: 1rem;
            text-align: center;

            > .icon {
                color: #FF5252;
                font-size: 5rem;

                .is-active {
                    color: #00E676;
                }
            }
        }

        .questionContainer {
            white-space: normal;

            height: 100%;
            width: 100%;

            .optionContainer {
                margin-top: 12px;
                flex-grow: 1;

                .option {
                    border-radius: 290486px;
                    padding: 9px 18px;
                    margin: 0 18px;
                    margin-bottom: 12px;
                    transition: $trans_duration;
                    cursor: pointer;
                    background-color: rgba(0, 0, 0, 0.05);
                    color: rgba(0, 0, 0, 0.85);
                    border: transparent 1px solid;

                    &.is-selected {
                        border-color: rgba(black, 0.25);
                        background-color: white;
                    }

                    &:hover {
                        background-color: rgba(0, 0, 0, 0.1);
                    }

                    &:active {
                        transform: scaleX(0.9);
                    }
                }
            }

            .questionFooter {
                background: rgba(0, 0, 0, 0.025);
                border-top: 1px solid rgba(0, 0, 0, 0.1);
                width: 100%;
                align-self: flex-end;

                .pagination {
                    //padding: 10px 15px;
                    margin: 15px 25px;
                }
            }
        }
    }

    .pagination {
        display: flex;
        justify-content: space-between;
    }

    .button {
        padding: 0.5rem 1rem;
        border: 1px solid rgba(0, 0, 0, 0.25);
        border-radius: 5rem;
        margin: 0 0.25rem;

        transition: 0.3s;

        &:hover {
            cursor: pointer;
            background: #ECEFF1;
            border-color: rgba(0, 0, 0, 0.25);
        }

        &.is-active {
            background: $primary_color;
            color: white;
            border-color: transparent;

            &:hover {
                background: darken($primary_color, 10%);

            }
        }
    }

    @media screen and (min-width: 769px) {
        .questionBox {
            align-items: center;
            justify-content: center;

            .questionContainer {
                display: flex;
                flex-direction: column;
            }
        }
    }

    @media screen and (max-width: 768px) {
        .sidebar {
            height: auto !important;
            border-radius: 6px 6px 0px 0px;
        }
    }

</style>
